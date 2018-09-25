Where is the configuration file of Nginx/Apache? Where is the PHP configuration file? What is the max body size of upload file? These are the repeated questions that I have listened/answered several times.

Actually, these questions mostly can be answered with **default** things, below are what I have collected when working with server stuffs. Hope they can help you on getting answer faster. Please keep in mind that they are **default** things, they are not always same as your current system.

### Nginx

These are 2 main installation types that we often meet:

##### Normal installation (via apt-get, yum, etc.)

- Main directory: /etc/nginx
- Main configuration file: /etc/nginx/nginx.conf
- Virtual host configuration directory: /etc/nginx/sites-available
- Max body size (normally considered as max uploaded file size, not correct but acceptable): 1 MB

##### Compile-from-source installation via Phusion Passenger

- Main directory: /opt/nginx
- Main configuration file: /opt/nginx/conf/nginx.conf
- Virtual host configuration: virtualhost is often added directly to **/opt/nginx/conf/nginx.conf**. You can add additional **.conf** file to **/opt/nginx/conf** directory too
- Max body size (normally considered as max uploaded file size, not correct but acceptable): 1 MB

### Apache

On Debian-based systems (e.g: Ubuntu, Debian, ApLinux, etc), its name is apache, on Redhat-based systems (e.g: Redhat, CentOS, Amazon Linux, etc), its name is httpd. Below information is for Debian-based systems, you can replace **apache2** with **httpd** for Redhat-based systems

- Main directory: /etc/apache
- Main configuration file: /etc/apache2/apache2.conf
- Virtual host configuration directory: /etc/apache2/sites-available
- Max body size (LimitRequestBody): unlimited

### MySQL

- Configuration file: multiple files, loaded by below order
    - /etc/my.cnf
    - /etc/mysql/my.cnf
    - /usr/etc/my.cnf
    - ~/.my.cnf
- Super user: root
- Port: 3306

### PostgreSQL

- Configuration file: /etc/postgresql/[VERSION]/main/pg_hba.conf, e.g: /etc/postgresql/9.5/main/pg_hba.conf
- Best way to find configuration file path is using SQL command: `SHOW hba_file;`
- Super user: postgres
- Port: 5432

### MongoDB

- Configuration file: no default file. Need to point to your customized configuration file if needed by start mongod with `mongod --config /etc/mongodb.conf`
- DB path: /data/db
- Find real DB path by running command in mongo CLI
    - db.serverCmdLineOpts().parsed.dbpath // MongoDB 2.4 and older
    - db.serverCmdLineOpts().parsed.storage.dbPath // MongoDB 2.6+
- Super user: nothing. MongoDB has no authentication by default. You can add a "super user" by adding a user with role **root** (version: 2.6+)
- Ports:
    - 27017 (for mongod, you mostly use this)
    - 27018 (for mongod with sharding/clustering)

### Redis

- Port: 6379
- Socket: /tmp/redis.sock
- Logs: /var/log/redis.log
- PID: /var/run/redis.pid
- Configuration directory: /etc/redis
- Configuration file: /etc/redis/redis.conf

### Postfix

- Main configuration directory: /etc/postfix
- Master configuration file: /etc/postfix/master.cf
- Main configuration file: /etc/postfix/main.cf
- Mail size limit: 10MB
- Show current configurations: run `postconf`

### Dovecot

- Main configuration directory: /etc/dovecot
- Main configuration file: /etc/dovecot/dovecot.conf
- Mail logs: /var/log/mail.log
- Mail location: nothing, automatically choose location based on each user. It will look at `~/Maildir`, `/var/mail/username`, `~/mail` and `~/Mail`. You should explicitly set mail location to centralize them
- Show current configurations: run `doveconf -a`

### WHM/cPanel

- Restart service: `/usr/local/cpanel/scripts/restartsrv_<service name>`. E.g: `/usr/local/cpanel/scripts/restartsrv_apache`, `/usr/local/cpanel/scripts/restartsrv_httpd`, `/usr/local/cpanel/scripts/restartsrv_imap`, `/usr/local/cpanel/scripts/restartsrv_mysql`, etc. See more at [WHM Scripts](https://documentation.cpanel.net/display/74Docs/WHM+Scripts)
- Port: 2087 for WHM, 2083 for cPanel
- Update WHM license: run `/usr/local/cpanel/cpkeyclt`

### Plesk

- PHP ini file: /usr/local/psa/admin/conf/php.ini
- Main configuration file: /etc/sw-cp-server/applications.d/plesk.conf
- Port: 8443