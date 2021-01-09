# mysql.md

This document will describe how to quickly setup a MySQL instance on your 
laptop or workstation.

Quite possibly the most convenient way to run a MySQL relational database
is to run a MySQL image in Docker. This does require that the system be
able to run Docker natively. On Windows, a VM instance of Linux with Docker
is a possibility. You may need to establish a bridged network to the VM 
however, but it is achievable.

With Docker compose a viewable configuration with volume persistence can 
be an easy way to just have a relational database for local development.

Here's a tested Docker compose configuration for MySQL.

```
version: '3.2'
services:

  mysql: # https://hub.docker.com/_/mysql
    image: mysql
    ports:
      - 3306:3306 # defaul mysql port (JDBC access)
    environment:
      MYSQL_ROOT_PASSWORD: mysqlAdmin123

  adminer: # https://hub.docker.com/_/adminer
    image: adminer
    ports:
      - 8088:8088
```

Note that the image does not prescribe a particular version. For local
development it should not matter what the version is. The important 
question is "is the production use of MySQL or MariaDB the right choice
for your project's needs, i.e. will MySQL or MariaDB be the production
choice?" Again, this Docker image is a tool to help during development.
If e.g. Mongo will be used in production, MySQL would not be a good
choice here for development.

Here's some SQL to refresh your memory while your setting up your new
MySQL Docker image.

```
/* Create new service database */
CREATE DATABASE IF NOT EXISTS `service`; /* Character set? */
USE `service`;

/* table-> api */
DROP TABLE IF EXISTS `api`;

 /* 
  * API
  */
CREATE TABLE `api` (
  `id`                int(6) unsigned     NOT NULL AUTO_INCREMENT,  
  `artifact_id`       varchar(40)         NOT NULL,                 
  `api_type`          varchar(10)         NOT NULL,
  `api_path`          varchar(80)         NOT NULL,                 -- /api/v1/resource/health
  PRIMARY KEY (`id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=latin1;

INSERT INTO `api` 
  (`artifact_id`, `api_type`, `api_path`) 
  VALUES (
    'my-shiny-newapi',
    'REST',
    '/api/v1/shinyone');

    
```