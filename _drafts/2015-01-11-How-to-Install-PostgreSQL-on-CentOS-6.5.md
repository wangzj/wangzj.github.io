---
layout: post
title: "How to Install PostgreSQL on CentOS 6.5"
date: 2015-01-11 22:32:33
categories: PostgreSQL
---

Before we install PostgreSQL, we should get over the dependencies.

    root $ yum -y install gcc-c++ zlib-devel readline-devel

### Build and Install

The following commands will install PostgreSQL into `/usr/local/pgsql`.

    root $ wget https://ftp.postgresql.org/pub/source/v9.4.0/postgresql-9.4.0.tar.gz
    root $ cd postgresql-9.4.0
    root $ ./configure
    root $ make && make install

For convenience, we'd better add related commands to `PATH` variable. Edit file
`/etc/profile`, and append the following line:

    export PATH=$PATH:/usr/local/pgsql/bin

### Initialize

Add a dedicated user `postgres`,

    root $ groupadd postgres
    root $ useradd postgres -g postgres

Now we will initialize the database server.

    root $ mkdir -p /var/lib/postgres
    root $ chown -R postgres:postgres /var/lib/postgres
    root $ chown -R postgres:postgres /usr/local/pgsql

    root $ su - postgres
    postgres $ /usr/local/pgsql/bin/initdb --encoding=utf8 -D /var/lib/postgres
    postgres $ exit

### Daemonize

Assume that $SRC_DIR represents the directory path we put sources of
PostgreSQL.

    root $ cp $SRC_DIR/postgresql-9.4.0/contrib/start-scripts/linux /etc/init.d/postgresql
    root $ chmod +x /etc/init.d/postgresql
    root $ chkconfig --add postgresql

Edit the start script, 

    PGDATA="/var/lib/postgres"

Then we could start the service,

    root $ service postgresql start

### Connecting to the Postgres Databases

For security, we should modify password of the user `postgres`.

    root $ psql -U postgres
    postgres=# ALTER USER postgres PASSWORD '123456'
    postgres=# \q

To allow external connections, edit the configuration file `pg_hba.conf`
located in `/var/lib/postgres`.

    host all all 0.0.0.0/0 md5

Edit `/var/lib/postgres/postgresql.conf`,

    - listen_addresses = 'localhost'
    + listen_addresses = '*'