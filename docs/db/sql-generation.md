---
sidebar_position: 3
sidebar_label: SQL Generation
title: SQL Generation
---

```bash
$ ./CSV_App sql
```

- This generates the SQL file in `data/db.sql` for the given CSV files
- CSV Data is validated against the constraints provided in `schema.json`
- Checkout the sample SQL file [here](https://github.com/mainlycricket/CSV_App/blob/main/data/db.sql)
- Run following commands to create database and execute the `db.sql` file

```bash
$ psql -h localhost -U postgres -c 'CREATE DATABASE "DB_Name"'
$ psql -h localhost -U postgres -d "DB_Name" -f data/db.sql
```

- `data/appConfig.json` is also generated in this step.
- Checkout the sample file [here](https://github.com/mainlycricket/CSV_App/blob/main/data/appConfig.json)
