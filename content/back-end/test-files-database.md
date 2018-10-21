---
title: "Populate Database w/ Test Files"
date: 2018-09-24
---

Testing often requires a populated database to work with. This tutorial explains how to write a write scripts which populate the database with test documents specified in files you will create.

## Setup

Make sure that MongoDB is set up and `mongod` is running. Review the structure of your collections. For this tutorial, we will be using users as our example, with the following schema:

{{< highlight js >}}
const mongoose = require('../db');

// Format of the mongoDB collection
const userSchema = mongoose.Schema({
  fullName: {
    type: String,
    required: true,
  },
  username: {
    type: String,
    required: true,
    index: {
      unique: true,
    },
  },
});

// Creating the mongoose model
const User = mongoose.model('User', userSchema);

module.exports = User;
{{< / highlight >}}

## Creating DB Test files

In your repository folder, create a new folder titled `DatabaseTestFiles`. In this folder, create a new file called `UserTest.json`. Line by line, create a new instance of a user. Here is an example:

{{< highlight json >}}
{"fullName":"John Doe","username":"19johnd"}
{"fullName":"Mary Molly","username":"17marym"}
{{< / highlight >}}

Note the lack of commas between objects.

#### A note on implementation

We are only using a model with two String fields. More models and fields could be added in the same fashion. If a reference field is added, there are two possibilities that could be implemented. The first would be to generate the ids in the database test files, and then reference those ids them in the other document. The other would be to generate documents for both collections separately, then randomly pair them together (depending on the direction of the reference). 

## Creating scripts

If it does not already exist, add a folder to your repository folder titled `scripts`.

### Unpopulate script

In your `scripts` folder, add a file titled `unpopulate.js`.
Add the following `require` block to the beginning of your code:

{{< highlight js >}}
const fs = require('fs');
const readline = require('readline');
const mongoose = require('../db');
const User = require('../models/user');
{{< / highlight >}}

If you were to add another model to the populate script, you would add its `require` reference here.
Then, add the `parseUsers` function. 

{{< highlight js >}}
function parseUsers() {
  return new Promise((resolve, reject) => {
    const mongoTransactions = [];
    const userRl = readline.createInterface({
      input: fs.createReadStream(`${__dirname}/../DatabaseTestFiles/UserTest.json`),
    });
    userRl.on('line', (data) => {
      mongoTransactions.push(
        User.findOne(JSON.parse(data))
          .then((user) => {
            if (!user) throw new Error(`User ith username ${JSON.parse(data).username} does not exist`);
            return user;
          })
          .then(user => user.remove())
          .then(() => console.log(`Removed user with username ${JSON.parse(data).username}`))
          .catch((err) => {
            console.log(`Error removing user from database: ${err}`);
          })
      );
    });
    userRl.on('close', () => {
      Promise.all(mongoTransactions)
        .then(resolve)
        .catch(reject);
    });
  });
}
{{< / highlight >}}

If you wanted to reference another model in User, for example 'Groceries', you would make a new, similar function but called `deleteGroceriesOfUsers` and which would take the parameter `user` and delete all of the groceries that that user referenced. Then, directly after this code block:

{{< highlight js >}}
.then((user) => {
  if (!user) throw new Error(`User ith username ${JSON.parse(data).username} does not exist`);
  return user;
})
{{< / highlight >}}

Add another `.then()` statement as so:

{{< highlight js >}}
.then(user => { deleteGroceriesOfUser(user) })
{{< / highlight >}}

Finally, at the end of the script, add this execution statement:

{{< highlight js >}}
if (module.parent) {
  module.exports = parseUsers();
} else {
  parseUsers().then(mongoose.disconnect);
}
{{< / highlight >}}

This checks to see if this code is being run by another script, which will be important for the `populate.js` script.

### Populate script

In your `scripts` folder, add a file titled `populate.js`. Add the following `require` block to the beginning of your code:

{{< highlight js >}}
const fs = require('fs');
const readline = require('readline');
const mongoose = require('../db');
const User = require('../models/user');
{{< / highlight >}}

If you were to add another model to the populate script, you would add its `require` reference here. Then, add the `parseUsers` function:

{{< highlight js >}}
function parseUsers() {
  const mongoTransactions = [];
  const users = [];
  const userRl = readline.createInterface({
    input: fs.createReadStream(`${__dirname}/../DatabaseTestFiles/UserTest.json`),
  });
  userRl.on('line', (data) => {
    const user = new User(JSON.parse(data));
    mongoTransactions.push(
      user.save().then((doc) => {
        users.push(doc);
      }).then(() => console.log(`Created user with username ${JSON.parse(data).username}`))
      .catch((err) => {
        console.log(`Error saving user to database: ${err}`);
      })
    );
  });
}
{{< / highlight >}}

This function goes line-by-line through the database test file and creates a new document for each.

If you wanted to reference another model in User, for example 'Groceries', you would make a new, similar function but called `parseGroceries` and which would take the parameter `users`. Then, append the following code inside of the `parseUsers` function:

{{< highlight js >}}
userRl.on('close', () => {
  Promise.all(mongoTransactions).then(users => {
    parseGroceries(users);
  });
});
{{< / highlight >}}

Finally, at the end of your code, add this statement:

{{< highlight js >}}
require('./unpopulate').then(parseUsers);
{{< / highlight >}}

This makes sure that the database is depopulated before repopulating it. 

<br><br>