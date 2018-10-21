---
title: "Access MongoDB with MongoDB Compass"
date: 2018-09-24
---

HarkerDev projects often (if not always) use MongoDB as the main database system. This guide will explain how to use MongoDB's GUI to access the database directly.

# Installing MongoDB Compass

## MongoDB

First, make sure that mongodb is installed and running `mongod` on the device you wish to connect to. This setup is detailed in our guide on [[Setting Up a Server|https://harkerdev.github.io/backend/setting-up-a-server]].

## Compass

Then, [[install MongoDB Compass|https://www.mongodb.com/download-center?filter=enterprise#compass]] on the device you are using to connect.

# Using MongoDB Compass

## Opening MongoDB Compass

MongoDB Compass runs as an application on your computer, so once you download it, open it (and move it to the Applications folder if applicable). 

## Connecting to server

Unless specified, most HarkerDev projects run on the default ports. This way, hostname, port, authentication, and replica set name do not have to be specified. To connect to a new server, click 'New Connection' on the left side of the interface.

### Connecting to local server

If you are connecting to a MongoDB instance running on the same computer as the one accessing it, then no other fields have to be specified. Click 'Connect' to start the connection.

### Connecting to remote server

If you are connecting to a remote server, under the SSH Tunnel field, select 'Use Password'. Then, enter the following:

- Domain (or IP Address) of the server in the 'SSH Hostname' field
- Server login username in the 'SSH Username' field
- Server login password in the 'SSH Password' field

Once these fields are filled in, click 'Connect'.

For context, the SSH Tunnel first connects into the server through SSH then connects to the MongoDB server locally. This does NOT connect to the MongoDB port but rather the SSH port.

### Adding to favorites

If you plan on often connecting to the MongoDB, you can make a favorite so that you do not have to fill in the fields every time you wish to connect to the server. To do this, fill in all the necessary fields but do not click 'Connect'. In the 'Favorite Name' field, fill in the name to refer to the server that you are connecting to. Then, click 'Create Favorite'.

## Using the GUI

Once logged in, you have the ability to interface with the databases on the specified server.

### Connecting to a Database

On the left side, there will be a list of databases that are hosted on the server. Usually on production servers,  only one main database will be used, with a name often similar to the application name (e.g. Course Planner => `courseplanner`). To access that database, click on it.

### Viewing a Collection

Under the database you are connected to, a list of collections should appear. To begin, click on the name of the collection you wish to manipulate. 

### Filtering a Collection
To filter a collection, use the filter at the top of the GUI. MongoDB has [[their own tutorial|https://docs.mongodb.com/v3.2/tutorial/query-documents/]] on queries, but here are a few examples:

#### Syntax
To filter the collection to documents that only have a property with a certain value, enter `{ "<property>": "<value>" }` into the filter, replacing `<property>` with the property and `<value>` with the value. For example, to query documents which have the `username` property set to `19johnd`, use the query:

{{< highlight json >}}
{ "username": "19johnd" }
{{< / highlight >}}

#### `or` Query

To have a filter which queries documents which *EITHER* have `property1` set to `value1` *OR* `property2` set to `value2`, use the query: 

{{< highlight json >}}
{ $or: [
  { "property1": "value1" }, 
  { "property2": "value2" },
]}
{{< / highlight >}}

#### `and` Query

To have a filter which queries documents which *BOTH* have `property1` set to `value1` *AND* `property2` set to `value2`, use the query: 

{{< highlight json >}}
{ $and: [
  { "property1": "value1" }, 
  { "property2": "value2" },
]}
{{< / highlight >}}

#### ObjectId Query

MongoDB's ObjectId's are be used to give a unique id to a document. They are usually either the value of the `_id` field or the value of a field which references other documents. To query documents which have the `_id` property set to ObjectId("5b1d"), use the query:

{{< highlight json >}}
{ "_id": { $oid: "5b1d" } }
{{< / highlight >}}

### Manipulating a document

Once documents are filtered, you can hover over a document for options (in the top right corner of the document). Compass allows you to edit the document (pencil icon) duplicate the document (multiple windows icon) or delete the document (trash bin icon).

#### Editing a fields

When editing a document, Compass allows allows you to modify fields. When hovering over a field, you can click on the value to allow you to edit that field. 

#### Adding/removing fields 

To delete a field, click the 'x' icon on the left side of the row. To add a field, click on the '+' icon on the left side of any row to add a field after it.

## More information

For more information, visit [[MongoDB's official Compass tutorial|https://docs.mongodb.com/compass/master/]].

<br><br>