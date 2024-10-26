---
layout: post
title: "Quick Guide to PostgreSQL Setup and Role Management on macOS"
date: 2024-10-25
categories: [Database]
tags: [PostgreSQL]
---

**Quick Guide to PostgreSQL Setup and Role Management on macOS**

###Start PostgreSQL service
brew services start postgresql

###Access PostgreSQL
psql postgres 

###To View existing roles
postgres=# \du

###Create a new role named postgres with login privileges and a password:
postgres=# CREATE ROLE postgres WITH LOGIN PASSWORD 'postgres'

###Grant the role database creation permissions:
postgres=# ALTER ROLE postgres CREATEDB;
postgres=# ALTER ROLE postgres CREATEROLE;

###Connect as the new role
psql postgres -U postgres 

###Create a new database
postgres=> CREATE DATABASE databasename

###Grant all privileges on the database to a specific user
postgres=> GRANT ALL PRIVIELEGES ON DATABASE databasename TO username

###List all databases:
postgres=> \list;

###Connect to your database
postgres=> \connect databasename;