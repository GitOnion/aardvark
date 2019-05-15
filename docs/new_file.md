HackMD Enterprise Edition - Installation And Configuration Guide
===
###### tags: `docs` `deploy`

[TOC]

## Introduction

HackMD Enterprise Edition will be delivered with Docker images, you will need Docker engine to run HackMD Enterprise Edition.

While CodiMD supports many database system, HackMD Enterprise Edition only supports PostgreSQL for better stability, you could follow this guide to migrate your database.

Before you start, you need to login to our Docker registry to pull the hackmd-ee Docker image. you can find the credentials you need in the email we sent.


### Hardware Requirement

* **CPU**: minimal 1 cores, 2GHz 
* **Memory**: minimal 2 GB
* **Disk**: free space at least 20GB

### Software Requirement

* Docker Engine CE/EE 18.03 or higher on Liunx x64 Architecture
* Docker Client CE/EE 18.03 or higher on Linux x64 Architecture

### Networking

* All HTTP traffic will go through port 3000.

:::warning
If you want to use **HTTPS** instead, you can configure a reverse proxy on the host's web server (i.e., Apache or nginx) and install your SSL Certificate on the web server. [More detail here.](#Setting-Reverse-Proxy-with-Nginx-for-HTTPS)
:::

## Installation

:::danger
If you are already using`HackMD Community Edition` and you want to migrate the existing database, please make sure the database is running PostgreSQL 9.6 or higher. Otherwise, you should follow [migration steps](#Migrate-HackMDCE-111-Database-to-HackMDEE) before you proceed further.
:::

We use [Docker Compose](https://docs.docker.com/compose/) to run HackMD Enterprise Edition. A`docker-compose.yaml` template is provided below, you can start with it and change environment variables to customize your service.

By default, all user-uploaded images are put into local file system. if you want to change your image storage, check out `Image Storage` section in`docker-compose.yaml`.

HackMD supports many OAuth providers, you can select which ones to use under `Login Method` section.


### Template of docker-compose.yaml
```yaml=
version: "3"
services:
  database:
    image: postgres:9.6.12
    environment:
      - POSTGRES_USER=<db_user_name>
      - POSTGRES_PASSWORD=<db_password>
      - POSTGRES_DB=<db_name>
    volumes:
      - database:/var/lib/postgresql/data
    restart: always

  cache:
    image: redis:4.0.13
    restart: always

  hackmd-ee:
    image: registry.dev.hackmd.io/company/hackmd-ee:1.3.11
    environment:
      # ----------------------------------------
      # |            Basic Setting             |
      # ----------------------------------------
      
      # database connection url
      - "HMD_DB_URL=postgres://<db_user_name>:<db_password>@database/<db_name>"
      
      # redis connection url
      - "HMD_REDIS_URL=redis://cache"

      # hackmd-ee service domain name or ip address
      # e.g. HMD_DOMAIN=doc.internal.dev
      # - "HMD_DOMAIN=192.168.1.100"

      # Enable "overview" so users can discover all
      # notes available to them.
      # This feature is very helpful for knowledge
      # discovery and sharing. However, because it 
      # was not available on CodiMD, if you migrate
      # your CodiMD database, your existing users'
      # notes might be exposed to other users
      # unintentionally.
      - "HMD_OVERVIEW=true"

      # Notes with what permissions would be shown on
      # the "overview" page for signed-in users with
      # AdvancedUser role? Options include:
      # freely, editable, limited, locked, protected, 
      # private
      - "HMD_INTERNAL_PUBLIC_PERMISSIONS=freely,editable,limited,locked,protected"

      # Notes with what permissions would be shown on
      # the "overview" page for guests?
      - "HMD_PUBLIC_PERMISSIONS=freely,editable,locked"

      # Default role given to signed-in users. Options
      # include: 'AdvancedUser', 'NoteAdmin',
      # 'UserAdmin', 'TeamAdmin', 'Superuser'
      - "HMD_DEFAULT_ROLE=AdvancedUser"

      # Allow user to share notes with specific users.
      - "HMD_ADVANCED_PERMISSION=true"

      # Enforce SSL only if you will setup a reverse proxy
      # for using TLS traffic.
      - "HMD_ENFORCE_SSL=true"
      
      # Enable comments on note
      - "HMD_COMMENTS=true"

      # ----------------------------------------
      # |            Image Storage             |
      # ----------------------------------------

      # Use local filesystem as default image storage.
      # Set this value to s3 / imgur / azure-storage /
      # google-cloud-storage and uncomment the services
      # settings below, if you want to store image at
      # respective service: 
      - "HMD_IMAGE_UPLOAD_TYPE=filesystem"

      # for AWS s3 
      # - HMD_S3_ACCESS_KEY_ID=
      # - HMD_S3_SECRET_ACCESS_KEY=
      # - HMD_S3_REGION=
      # - HMD_S3_BUCKET=
      # ----------------------------------------
      # for imgur
      # - HMD_IMGUR_CLIENTID=
      # ----------------------------------------
      # for azure storage
      # storage connection string
      # - HMD_AZURE_STORAGE_CONNSTR=
      # - HMD_AZURE_STORAGE_CONTAINER_NAME=
      # ----------------------------------------
      # for google cloud storage=
      # - HMD_GCLOUD_STORAGE_BUCKET=
      # - HMD_GCLOUD_STORAGE_KEYFILE=
      # - HMD_GCLOUD_STORAGE_EMAIL=

      # ----------------------------------------
      # |            Sign-in Method              |
      # ----------------------------------------
      # Allow user to register account via email address
      # and password. This has to be true for 
      # HMD_ALLOW_EMAIL_REGISTER_DOMAIN to take effect,
      # and allow those users who were invited with
      # HMD_EMAIL_INVITATION to register.
      - "HMD_ALLOW_EMAIL_REGISTER=false"

      # Allow user to sign-in with email address and
      # password. This has to be true for 
      # HMD_EMAIL_INVITATION to take effect.
      - "HMD_EMAIL=true"

      # Allow inviting users using email
      - "HMD_EMAIL_INVITATION=true"

      # Restrict which email domain could register.
      - "HMD_ALLOW_EMAIL_REGISTER_DOMAINS="

      # ----------------------------------------
      # Facebook OAuth
      # - HMD_FACEBOOK_CLIENTID=
      # - HMD_FACEBOOK_CLIENTSECRET=
      # ----------------------------------------
      # Twitter OAuth
      # - HMD_TWITTER_CONSUMERKEY=
      # - HMD_TWITTER_CONSUMERSECRET=
      # ----------------------------------------
      # GitHub OAuth
      # - HMD_GITHUB_CLIENTID=
      # - HMD_GITHUB_CLIENTSECRET=
      # ----------------------------------------
      # GitLab OAuth
      # - HMD_GITLAB_BASEURL=
      # - HMD_GITLAB_CLIENTID=
      # - HMD_GITLAB_CLIENTSECRET=
      # - HMD_GITLAB_SCOPE=
      # ----------------------------------------
      # Dropbox OAuth
      # - HMD_DROPBOX_CLIENTID=
      # - HMD_DROPBOX_CLIENTSECRET=
      # - HMD_DROPBOX_APPKEY=
      # ----------------------------------------
      # Google OAuth
      # - HMD_GOOGLE_CLIENTID=
      # - HMD_GOOGLE_CLIENTSECRET=
      # - HMD_GOOGLE_APIKEY=
      # ----------------------------------------
      # LDAP/AD Sign-in
      # - HMD_LDAP_PROVIDERNAME=
      # - HMD_LDAP_URL=
      # - HMD_LDAP_BINDDN=
      # - HMD_LDAP_BINDCREDENTIALS=
      # - HMD_LDAP_TOKENSECRET=
      # - HMD_LDAP_SEARCHBASE=
      # - HMD_LDAP_SEARCHFILTER=
      # - HMD_LDAP_SEARCHATTRIBUTES=
      # - HMD_LDAP_TLS_CA=
      # ----------------------------------------
      # SAML Sign-in
      # - HMD_SAML_IDPSSOURL=
      # - HMD_SAML_IDPCERT=
      # - HMD_SAML_ISSUER=
      # - HMD_SAML_IDENTIFIERFORMAT=
      # - HMD_SAML_GROUPATTRIBUTE=
      # - HMD_SAML_EXTERNALGROUPS=
      # - HMD_SAML_REQUIREDGROUPS=
      # - HMD_SAML_ATTRIBUTE_ID=
      # - HMD_SAML_ATTRIBUTE_USERNAME=
      # - HMD_SAML_ATTRIBUTE_EMAIL=
      # - HMD_SAML_ATTRIBUTE_DISPLAY_NAME=

    # Container port maps to Host port
    ports:
      - "3000:3000"

    # Host data storage path maps to Container mounting 
    # point
    volumes: 
      - hackmd_data:/hackmd/public/uploads
    restart: always

# Declaring volume name variables, leave them blank.
volumes:
  database: {}
  hackmd_data: {}

```

### Bash Command for Installing HackMD(EE):

1. Login to HackMD's Docker registry
```bash=
$ docker login registry.dev.hackmd.io
Username: example_username
Password:
Login Succeeded
```

2. Pull the HackMD(EE) image
```bash=
docker pull registry.dev.hackmd.io/xcoo/hackmd-ee:1.3.8
```

3. Run the services
```bash=
$ docker-compose up -d
```

### Setting Reverse Proxy with Nginx for HTTPS
If you are setting up a reverse proxy for HTTPs traffic, be sure to enforce this variable in the `docker-compose.yml` file:

```bash=
HMD_ENFORCE_SSL=true
```

See below for an example nginx config file:
```nginx=
upstream @hackmd {
    server hackmd-ee:3000;
    keepalive 300;
}

map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
    listen 80;
    server_name hackmd-test.xcoo.jp;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name hackmd-test.xcoo.jp;

    ssl_certificate /etc/ssl/xcoo.jp/xcoo.jp.full.crt;
    ssl_certificate_key /etc/ssl/xcoo.jp/xcoo.jp.key;

    access_log /dev/stdout;
    error_log /dev/stderr;

    location / {
      proxy_http_version 1.1;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;
      client_max_body_size 8192m;
      proxy_max_temp_file_size 8192m;
      proxy_read_timeout 300;
      proxy_connect_timeout 300;
      proxy_pass http://@hackmd;
    }
}
```


## Regular Maintenance
### Database Backup
Start your Docker container and SSH into the container shell, then use pg_dump command to backup all data.

```
docker-compose exec database pg_dump <db_name> -U <user_name>  > backup.sql
```

### Database Restore
Use psql command to restore data

```
cat backup.sql | docker exec -i database psql -U <user_name>
```

## Migrate HackMD(CE) 1.1.1 Database to HackMD(EE)

Follow these steps to migrate HackMD(CE) 1.1.1 to HackMD(EE):

1. Export HackMD(CE) database into a csv file (without the title row)
2. Create a new PostgreSQL database and apply HackMD(EE) database schema
3. Import the CSV file to PostgreSQL
4. Complete

### 1. Export HackMD(CE) Database into a CSV File

If you are using HackMD(CE) 1.1.1, you only need to export four tables, namely: Authors, Notes, Revisions, and Users. You can use the [SELECT ... INTO OUTFILE](https://dev.mysql.com/doc/refman/8.0/en/select-into.html) SQL syntax in MySQL/MariaDB to export the table rows to csv file. (Note: we don't need the title row.)

#### Example:

Here's a sql script to export data to `/backup` directory.

```
SELECT * FROM Authors INTO OUTFILE '/backup/authors.csv' FIELDS ENCLOSED BY '"' TERMINATED BY ',' ESCAPED BY '\\' LINES TERMINATED BY '\r\n';
SELECT * FROM Notes INTO OUTFILE '/backup/notes.csv' FIELDS ENCLOSED BY '"' TERMINATED BY ',' ESCAPED BY '\\' LINES TERMINATED BY '\r\n';
SELECT * FROM Revisions INTO OUTFILE '/backup/revisions.csv' FIELDS ENCLOSED BY '"' TERMINATED BY ',' ESCAPED BY '\\' LINES TERMINATED BY '\r\n';
SELECT * FROM Users INTO OUTFILE '/backup/users.csv' FIELDS ENCLOSED BY '"' TERMINATED BY ',' ESCAPED BY '\\' LINES TERMINATED BY '\r\n';
```

### 2. Create a New PostgreSQL Database and Apply the HackMD(EE) Database Schema

Start the PostgreSQL service from within the Host.

```bash=
docker-compose up -d database
```

Run below sql script to create a new PostgreSQL database and Apply the HackMD(EE) Database Schema

#### PostgreSQL Schema Script

```sql
--
-- HackMD(CE) 1.1.1 to HackMD(EE) 1.1
--

SET statement_timeout = 0;
SET lock_timeout = 0;
SET idle_in_transaction_session_timeout = 0;
SET client_encoding = 'UTF8';
SET standard_conforming_strings = on;
SELECT pg_catalog.set_config('search_path', '', false);
SET check_function_bodies = false;
SET client_min_messages = warning;
SET row_security = off;

SET default_tablespace = '';
SET default_with_oids = false;

CREATE TABLE public."Authors" (
    id integer NOT NULL,
    color character varying(255),
    "noteId" uuid,
    "userId" uuid,
    "createdAt" timestamp with time zone,
    "updatedAt" timestamp with time zone
);

CREATE SEQUENCE public."Authors_id_seq"
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public."Authors_id_seq" OWNED BY public."Authors".id;

CREATE TABLE public."Notes" (
    id uuid NOT NULL,
    "ownerId" uuid,
    content text,
    title text,
    "createdAt" timestamp with time zone,
    "updatedAt" timestamp with time zone,
    shortid character varying(255) DEFAULT '0000000000'::character varying NOT NULL,
    permission character varying(255) DEFAULT 'private'::character varying NOT NULL,
    viewcount integer DEFAULT 0,
    "lastchangeuserId" uuid,
    "lastchangeAt" timestamp with time zone,
    alias character varying(255),
    "savedAt" timestamp with time zone,
    authorship text,
    "deletedAt" timestamp with time zone
);

CREATE TABLE public."Revisions" (
    id uuid NOT NULL,
    "noteId" uuid,
    patch text,
    "lastContent" text,
    content text,
    length integer,
    "createdAt" timestamp with time zone,
    "updatedAt" timestamp with time zone,
    authorship text
);

CREATE TABLE public."SequelizeMeta" (
    name character varying(255) NOT NULL
);

CREATE TABLE public."Users" (
    id uuid NOT NULL,
    profileid character varying(255),
    profile text,
    history text,
    "createdAt" timestamp with time zone,
    "updatedAt" timestamp with time zone,
    "accessToken" character varying(255),
    "refreshToken" character varying(255),
    email text,
    password text
);

COPY public."SequelizeMeta" (name) FROM stdin;
20150504155329-create-users.js
20150508114741-create-notes.js
20150702001020-update-to-0_3_1.js
20150915153700-change-notes-title-to-text.js
20160112220142-note-add-lastchange.js
20160420180355-note-add-alias.js
20160515114000-user-add-tokens.js
20160607060246-support-revision.js
20160703062241-support-authorship.js
20161009040430-support-delete-note.js
20161201050312-support-email-signin.js
\.

ALTER TABLE ONLY public."Authors"
    ADD CONSTRAINT "Authors_pkey" PRIMARY KEY (id);

ALTER TABLE ONLY public."Notes"
    ADD CONSTRAINT "Notes_pkey" PRIMARY KEY (id);

ALTER TABLE ONLY public."Revisions"
    ADD CONSTRAINT "Revisions_pkey" PRIMARY KEY (id);

ALTER TABLE ONLY public."SequelizeMeta"
    ADD CONSTRAINT "SequelizeMeta_pkey" PRIMARY KEY (name);

ALTER TABLE ONLY public."Users"
    ADD CONSTRAINT "Users_pkey" PRIMARY KEY (id);

ALTER TABLE ONLY public."Users"
    ADD CONSTRAINT "Users_profileid_key" UNIQUE (profileid);

CREATE UNIQUE INDEX notes_alias ON public."Notes" USING btree (alias);

CREATE UNIQUE INDEX notes_shortid ON public."Notes" USING btree (shortid);

```

### 3. Import the CSV File to PostgreSQL

Use meta-command of psql: `\COPY` to import csv file into the PostgreSQL database.

:::warning
The order of column names in the `\COPY` command has to be exactly the same as that in the CSV file, otherwise the migration might fail.
:::

#### Data Import Script:
```
\COPY "Notes" ("id", "ownerId", "content", "title", "createdAt", "updatedAt", "shortid", "permission", "viewcount", "lastchangeuserId", "lastchangeAt", "alias", "savedAt", "authorship", "deletedAt") FROM '/backup/notes.csv' WITH (FORMAT csv, DELIMITER ',' , NULL '\N', QUOTE '"', ESCAPE '\');

\COPY "Authors" (id, color, "noteId", "userId", "createdAt", "updatedAt") FROM '/backup/authors.csv' WITH (FORMAT csv, DELIMITER ',' , NULL '\N', QUOTE '"', ESCAPE '\');

\COPY "Revisions" (id, "noteId", "patch", "lastContent", "content", "length", "createdAt", "updatedAt", "authorship") FROM '/backup/revisions.csv' WITH (FORMAT csv, DELIMITER ',' , NULL '\N', QUOTE '"', ESCAPE '\');

\COPY "Users" ("id", "profileid", "profile", "history", "createdAt", "updatedAt", "accessToken", "refreshToken", "email", "password") FROM '/backup/users.csv' WITH (FORMAT csv, DELIMITER ',' , NULL '\N', QUOTE '"', ESCAPE '\');

```

### 4. Complete

You are done with the database migration. Now you can follow [Installation](#Installation).