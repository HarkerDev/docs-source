---
title: "MongoDB Backup"
date: 2018-09-23
---

The more backups there are, the better. Thankfully, because of GitHub, we have multiple backups of the code in addition to the code on the server and on our own computers. However, MongoDB needs more work to set up for backups. This tutorial will guide you on how to make a backup procedure for MongoDB.

## Local Backup

A local backup is helpful because data can become corrupted or maliciously manipulated while in production. A local backup is a great tool for fixing this because the database can be restored from the local backup. However, this does not protect against total hard drive failure.

### Creating a Backup Script

In your home folder, create a script titled `backup` by entering the following command:

````bash
nano backup
````

After creating the file, append the following commands to it (replace `<database>` with the database you are using):

````ash
NOW=$(date +%Y-%m-%d_%H:%M:%S)
mongodump -d <database> -o ~/backups/$NOW.mdb
````

Then save the file (if using `nano` press control+x). Then, in your shell, give yourself permission to run it:

````bash
sudo chmod 500 ~/backup
````

To execute the script, run the following command:

````bash
~/backup
````

### Scheduling Periodic Script Execution

Although the `backup` script can be run manually, it would be more efficient for the script to run automatically. To schedule a periodic execution of the script, first run the following command:

````bash
crontab -e
````

Then scroll your cursor to the bottom of the file. Add a new line after the comments (comments start with #). Once you add the line, save the file by pressing control+x. Different periods are defined below, but you can also read [this in-depth guide](https://www.computerhope.com/unix/ucrontab.htm) on crontab.

#### Daily Execution

To add a daily execution at midnight, add the following line to the file (replace `<userfolder>` with your username):

````bash
0 0 * * * /home/<userfolder>/backup
````

#### Weekly Execution

To add a weekly execution on Sunday at midnight, add the following line to the file (replace `<userfolder>` with your username):

````ash
0 0 * * 0 /home/<userfolder>/backup
````
