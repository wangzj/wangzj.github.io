---
layout: post
title: "Practical PostgreSQL Database Administration Commands"
date: 2015-01-12 13:09:07
categories: PostgreSQL
---

    \c dbname  # switch database
    \l         # list databases
    \dt        # list tables
    \q         # quit

    pg_dump -st tblname dbname -h host -U user  # dump sql
    psql -d dbname -f script.sql              # import sql