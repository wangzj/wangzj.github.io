---
layout: post
title: "Configure Django with Postgresql"
date: 2015-01-09 11:47:25
categories: Python
imageuri: 3397b59b9b7524b8e8d29635f88159b7.jpg
---

### Requirements

We first upgrade python to version of 2.7. Following the instructions in
[[1][python]], we compile the source with options below:

    $ ./configure --prefix=/usr/local

But before performing this command, we should install the dependencies:

    $ yum -y install openssl-devel

Setuptools and pip are always helpful.

    root $ wget https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py
    root $ python ez_setup.py
    root $ easy_install pip


### Install django

Then we can use `pip` to install packages that are required by the project.

    root $ pip install psycopg2
    root $ pip install django

Note that `psycopg2` depends on postgres library. That's why we edit
`/etc/profile` and add the following line.

    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/pgsql/lib

On windows platform, we take pre-built binaries of psycopg2 from [2] rather
than using `pip` command.

### Configure django

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': 'panoramics',
            'USER': 'postgres',
            'PASSWORD': '123456',
            'HOST': 'localhost',
            'PORT': 5432,
        }
    }

####Reference

[[1][python]] https://github.com/h2oai/h2o/wiki/Installing-python-2.7-on-centos-6.3.-Follow-this-sequence-exactly-for-centos-machine-only <br>
[[2][psycopg2]] http://www.stickpeople.com/projects/python/win-psycopg/

[django-setup]: https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-django-with-postgres-nginx-and-gunicorn
[python]: https://github.com/h2oai/h2o/wiki/Installing-python-2.7-on-centos-6.3.-Follow-this-sequence-exactly-for-centos-machine-only
[psycopg2]: http://www.stickpeople.com/projects/python/win-psycopg/