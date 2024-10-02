---
sidebar_position: 1
sidebar_label: Getting Started
title: Getting Started
---

### Installing Requirements

Download and install the following if not already installed

- [Go](https://go.dev/dl/)
- [PostgreSQL](https://www.postgresql.org/download/)
- [GoImports](https://pkg.go.dev/golang.org/x/tools/cmd/goimports)

These commands should execute successfully:

```bash
$ go version
$ psql --version
$ goimports --help
```

### Setting-up the project

- Clone the [project](https://github.com/mainlycricket/CSV_App)
- Delete the existing `./app` directory
- Remove the files in `./data` directory and copy your CSV files here.

```bash
$ git clone git@github.com:mainlycricket/CSV_App.git
$ cd CSV_App
$ go build .
$ rm -r app
$ rm data/*
# copy csv files in ./data
```

### General Overview

1. Generate schema and examine it i.e. `data/schema.json`

```bash
$ ./CSV_App schema
```

2. Generate SQL file and seed the database

```bash
$ ./CSV_App sql
$ psql -h localhost -U postgres -c 'CREATE DATABASE "DB_Name"'
$ psql -h localhost -U postgres -d "DB_Name" -f data/db.sql
```

3. Examine `data/appConfig.json` and generate app

```bash
$ ./CSV_App app
$ cd app && ./setup.sh
```

4. Modify the .env and start the server

```bash
# modify .env and start the server
$ ./app
```

Continue reading to understand all the configurations available
