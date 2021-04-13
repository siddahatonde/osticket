osticket - Installation
===============

# Introduction

Docker image for running version 1.14.3 of [OSTicket](http://osticket.com/).


It has a few modifications:

  * Documentation added, hurray!
  * Base OS image fixed to Alpine Linux
  * AJAX issues fixed that made original image unusable
  * Now designed to work with a linked [MySQL](https://registry.hub.docker.com/u/library/mysql/) docker container.
  * Automates configuration file & database installation
  * EMail support

OSTicket is being served by [nginx](http://wiki.nginx.org/Main) using [PHP-FPM](http://php-fpm.org/) with PHP 7.2.
PHP7's [mail](http://php.net/manual/en/function.mail.php) function is configured to use [msmtp](http://msmtp.sourceforge.net/) to send out-going messages.

The `setup/` directory has been renamed as `setup_hidden/` and the file system permissions deny nginx access to this
location. It was not removed as the setup files are required as part of the automatic configuration during container
start.

# Quick Start

Ensure you have a MySQL 5+ container running that OSTicket can use to store its data.

```bash
docker run --name osticket_mysql -d -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_USER=osticket -e MYSQL_PASSWORD=secret -e MYSQL_DATABASE=osticket mysql:5
```

Now run this image and link the MySQL container.

```bash
docker run --name osticket -d --link osticket_mysql:mysql -p 8080:80 campbellsoftwaresolutions/osticket
```

Wait for the installation to complete then browse to your OSTicket staff control panel at `http://localhost:8080/scp/`. Login with default admin user & password:

* username: **ostadmin**
* password: **Admin1**

Now configure as required. If you are intending on using this image in production, please make sure you change the
passwords above and read the rest of this documentation!

Note (1): If you want to change the environmental database variables on the OSTicket image to run, you can do it as follows.

```bash
docker run --name osticket -d -e MYSQL_ROOT_PASSWORD=new_root_password -e MYSQL_USER=new_root_user -e MYSQL_PASSWORD=new_secret -e MYSQL_DATABASE=osticket --link osticket_mysql:mysql -p 8080:80 campbellsoftwaresolutions/osticket
```

OSTicket automatically redirects `http://localhost:8080/scp` to `http://localhost/scp/`. Either serve this on port 80 or don't omit the
trailing slash after `scp/`!

