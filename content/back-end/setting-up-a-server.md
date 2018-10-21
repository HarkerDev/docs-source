---
title: "Setting Up A Server"
date: 2018-09-24
---

You'll be writing code for two different environments: development and production. The production environment is the server. The stuff on it should be free of errors, and ideally you don't want to be testing anything in production. The development environment is what you run on your machine. You should ideally test your changes in your development environment before deploying them to production.

## Development

Because you'll be the only one testing the application locally on your computer (or maybe somehow you could involve a few others), it is enough to only utilize one core of your machine and not worry about the application's response to load. For that reason, getting things set up locally doesn't involve too much.

### Installation

#### 1. Git

In order to clone the repository you'll need Git installed. There are a million guides (maybe not quite a million, but a good amount) on the internet on how to do this. [Here is one such guide.](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) If you're on Windows, want to use the command line, want to clone over https, and want to use Two Factor Authentication for GitHub you'll also want to use [Microsoft's Git Credential Manger for Windows](https://github.com/Microsoft/Git-Credential-Manager-for-Windows]). Oh yeah and if you don't want to use git's command line client (which is very powerful), there are various GUIs built for it, one such being [GitHub Desktop](https://desktop.github.com/).

#### 2. Node.js

In order to run the code you'll need Node.js (assuming we're using that as the programming language). Different projects will have different minimum versions depending on how long ago they were coded. If you don't know what version you'll need, consult with whoever set up the server for the project and see what version is on the server.

There are different ways of handling multiple Node.js versions. One such way is to install [`n`](https://github.com/tj/n), an application for managing these different versions. Unfortunately this is not supported on Windows. However, given that Node.js is mostly backwards-compatible, you may be able to get by with installing the latest version. Just be aware of new features not available in the version being used for the project.

#### 3. MongoDB

Most of our projects use MongoDB as a database. You can get that from [here](https://www.mongodb.com/download-center#community).

### Running

1. Open Terminal. If on macOS, and enter the command `mongod` in a new Terminal window. If on Linux, enter the command `sudo service mongod start`. If on Windows, first create a new folder called data with a folder inside called db on your local disk (usually the C drive). Then run mongod.exe to launch the database. You can do this by opening command prompt and entering the path to the executable (by default it is `"C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe"`).

2. In Terminal (If using macOS, open a new Terminal window), open the repository's directory, and enter the command `node index.js`, and press the `control` and `C` keys at the same time to stop it. To have the program run detached from the terminal (so it continues running after you exit the Terminal window), enter the command `nohup node index.js`. If on Windows, open command prompt, navigate to the repository's directory, and enter the command `node index.js`. Press the `control` and `C`at the same time to stop it.

3. Open `http://localhost:<port>` in your browser, where `<port>` is the port specified in your config file.

## Production

### Installation

In the production environment since performance matters we use more software to help handle the load of many students accessing an application at once. This will be discussed below.

1. Node.js

    We use Ubuntu as the OS. Node.js has a guide for installing Node.js on Ubuntu [here](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions).

2. Nginx

    Nginx is a web server that can serve static content as well as act as a proxy. It is much more efficient than doing anything in Node.js, therefore we use it to serve the static content for the site, manage TLS, as well as proxy the Node.js code. For most software installation on the Development server, DigitalOcean always has very good guides. They happen to publish ones on installing Nginx, so to find the one relevant to your Ubuntu version search for something like "nginx install ubuntu" and find the relevant link. For example [here](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04) is the one for 16.04. Now for this guide because Harker has its own firewall and we don't host with DigitalOcean you can ignore the part with `ufw`. Pretty much all you need to do as of writing is `sudo apt-get update` and `sudo apt-get install nginx`.

3. pm2

    Another thing that makes the application more performant in production is utilizing multiple cores. Node.js has ways to do this an the application, but pm2 makes this much easier and in facts prevents you from having to write any code for utilizing multiple cores. It also handle auto-restarting the application if there are any errors and saving log files. To install it follow the instructions [here](http://pm2.keymetrics.io/docs/usage/quick-start/#installation).

4. Certbot

    To prevent having to wait for Harker to give us SSL certificates, we use the Let's Encrypt service. In order to use it, we'll need an application for registering with said service. Again, DigitalOcean has a nice guide for this. As said before their guides are Ubuntu-version specific, but [here's](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04) the one for 16.04. You can skip the parts about setting up nginx and updating `ufw`. When you get to running the `certbot` commands, you'll need to have set up nginx and disabled https redirection (as we won't have a certificate yet). Make sure to complete the part titled "Updating Diffie-Hellman Parameters", but don't edit the config as that will already be handled. You'll also want to set up auto-renewal (which is covered in the guide).

5. MongoDB

    You can refer to [MongoDB's guide](https://docs.mongodb.com/v3.0/tutorial/install-mongodb-on-ubuntu/) for this.

### Setup

Nginx has two different directories for configuration: `/etc/nginx/sites-available/` and `/etc/nginx/sites-enabled/`. The configuration files are stored in `sites-available`, and they are symlinked (use `ln -s`) to `sites-enabled`. Grab a previous config file from `sites-available` (it's name will match the DNS name of the site), and put that in the same folder on the new server but with the appropriate name. You'll also want to copy over the files in `/etc/nginx/snippets`.

Then delete the default site symlink in `/etc/nginx/sites-available/` and symlink the new file in `site-available` to `sites-enabled`. You'll then want to edit the new file in `sites-available`, updating paths as well as names. You'll also want to disable HTTPS redirection before setting up Let's Encrypt (but remember to turn it back on).

You'll also need to clone the repo into the home folder. You'll need to set up SSH Forwarding for this and to clone the repo over SSH.

### Running

Use `sudo service mongod start` to start MongoDb, and `pm2 start app.js -i 0 --name "app"` (assuming `app.js` is the main file and you're in the directory you cloned) to start the main application for the first time. Later you can just use `pm2 start app`.

### Backups
Consider viewing [[this tutorial|https://github.com/DJMcoder/HarkerDevGuides/wiki/Creating-Backup-Procedures]] for an in-depth guide on creating a backup procedure.

<br><br>