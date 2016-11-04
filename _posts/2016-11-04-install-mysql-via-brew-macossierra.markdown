---
layout: post
title:  "Install mysql, via homebrew, on macOS"
date:   2016-11-04 13:48:15 +0000
categories: osx
---

After upgrading my machine to macOS Sierra I've found out that some of my apps were broken due to a crippled instalation of MySQL.

Gogling a bit, I've found out that people usually complain on the same errors while trying to install it using brew:

    Error: Can't connect to local MySQL server through socket '/tmp/mysql.sock'

    ERROR! MySQL server PID file could not be found!

    ERROR! The server quit without updating PID file

And all sorts of errors that showed that the mysql server was not able to start. On this small post I wish to try and give a quick method on how to properly set up mysql on a mac, with a few easy steps.

## Start clean

The first step is to start with a clean slate:

    josealves:mysql/ $ brew remove mysq

Then we simply prune old cellar packages, which should probably also clean up some space for you:

    josealves:mysql/ $ brew cleanup --force

By default, homebrew leverages [launchctl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/launchctl.1.html) to manage the machine services, so let us then clean it up a bit first:

    josealves:mysql/ $ launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

    josealves:mysql/ $ rm ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

And finally clean up any remaining errors or pid files from:

    josealves:mysql/ $ sudo rm -rf /usr/local/var/mysql

## A new beginning:

We can now start a fresh instalation by simply running:

    josealves:~/ $ brew install mysql

Then we initialize the daemon for the first time, adding a root user (with a temporary password) and a test table:

    josealves:~/ $ mysqld --initialize --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql

    2016-11-04T11:34:30.841682Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
    2016-11-04T11:34:30.844130Z 0 [Warning] Setting lower_case_table_names=2 because file system for /usr/local/var/mysql/ is case insensitive
    2016-11-04T11:34:31.242088Z 0 [Warning] InnoDB: New log files created, LSN=45790
    2016-11-04T11:34:31.301255Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
    2016-11-04T11:34:31.372550Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: a710d5c4-a282-11e6-a5fb-45c0f17a8e5a.
    2016-11-04T11:34:31.396521Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
    2016-11-04T11:34:31.684205Z 0 [Warning] CA certificate ca.pem is self signed.
    2016-11-04T11:34:31.756226Z 1 [Note] A temporary password is generated for root@localhost: i*V7ZRN#NBi1

Now, hopefully, we can start the server by simply running:

    josealves:~/ $ mysql.server start


The next step is to secure the mysql server (set a definitive root password, trigger root remote access, allow anonymous users, ...) by running their script at:

    josealves:~/ $ mysql_secure_installation

## At your service

You can now use brew to manage your fresh installed service by using:

    [sudo] brew services list
    List all running services for the current user (or root)

    [sudo] brew services start formula
    Install and start the service formula at login (or boot).

    [sudo] brew services stop formula|--all
    Stop the service formula after it was launched at login (or boot).

    [sudo] brew services restart formula
    Stop (if necessary), install and start the service formula at login (or boot).

    [sudo] brew services cleanup
    Remove all unused services.

So, as an example, we start the server:

    josealves:~/ $ brew services restart mysql

    josealves:~/ $ brew services list
    Name  Status  User      Plist
    mysql started josealves /Users/josealves/Library/LaunchAgents/homebrew.mxcl.mysql.plist

And you are now able to login to mysql successfully:

    josealves:~/ $ mysql -uroot -p
    Enter password:
    Welcome to the MySQL monitor.  Commands end with ; or \g.

I know it is a short quick guide, but I hope it helps on saving some time while trying to easily configure this service on your local machine.
