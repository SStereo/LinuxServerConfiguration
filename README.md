Linux Server Configuration
==========================

This is an example configuration of a Linux server with the purpose to host
python based web applications using PostGre or SQLite databases.

ACCESS
--------------------
1. Web access: http://35.153.73.73 on your browser (port 80)
2. SSH access: ip 35.153.73.73 with port 2200 using key authentication

FEATURES
--------
1. No remote login via root possible
2. All users require key authentication for remote accessed
3. Firewall only support http, ssh and ntp only
4. Web application runs under a separate user without admin access
5. Python web application runs on Apache with an wsgi interface
6. Dynamic URL rewrite with Apache to change IP to a domain URL to enable oauth

SOFTWARE INSTALLED
------------------
The following list contains all software packages (names in brackets) as well as
additional libraries installed:
- Ubuntu/trusty Linux (provided via AWS Lightsail)
- Apache web application server (package: apache2) as well as mods (libapache2-mod-wsgi)
- Python (package: python, python-dev, python-pip) as well as python
  libraries installed via pip (setuptools, flask, sqlalchemy, flask-sqlalchemy, request, passlib, request)
- Finger (finger)
- GIT (git)
- PostGreSQL database (postgresql, postgresql-client-common, postgresql-client-9.3)
  and related python libraries (psycopg2)
- SQLite database (sqlite3)

CONFIGURATION CHANGES
---------------------
- Generation of private keys using ssh-keygen for required users
- Created an additional user to run the web application (adduser)
- Keys enabled on server per user (~/.ssh/authorized_keys)
- Updating all software packages (apt-get update/upgrade)
- Configured & enabled firewall to only enable http, ssh and ntp (ufw allow/deny)
- Configured AWS instance firewall to enable non default port for ssh
- Configured ssh service to listen to no-default port (etc/ssh/sshd_conf)
- Configured ssh service to disable remote root login
- Configured time zone to UTC (dpkg-reconfigure tzdata)
- Installation of additional software (see section Software Installed)
- Provisioning of an example pyhton based web application named catalog

WEB SERVER CONFIGURATION (Apache)
- Install wsgi mod
- Enabled rewrite mod for dynamic ip to dns change (a2enmod rewrite)
- create folder for the web application owned by a user catalog (/var/www/catalog)
- Modified default virtual host file (/etc/apache2/sites-enabled/000-default.conf) to
  a) wsgi to point to /var/www/catalog/catalog.wsgi
  b) Dedicated daemon to run the app with the catalog user
  c) Enable the rewrite engine to change browser URL from IP to a xip.io domain
- cloned the git repository of the example app into the app folder
- created a wsgi file that adds app folder to system path and imports the application from the main python module

DATABASE SERVER CONFIGURATION (PostGreSQL)
- Create a database user that matches the os user for the web app (catalog)
- Create a database for the web application (catalog)

3rd PARTY RESOURCES
-------------------
- Stackoverflow
- Ubuntu Packages (https://packages.ubuntu.com/)
- Ubuntu Manuals (http://manpages.ubuntu.com/)
- Flask documentation for wsgi (http://flask.pocoo.org/docs/0.10/deploying/mod_wsgi/)

CONTACT
-------
You can contact me on GitHub under SStereo.
