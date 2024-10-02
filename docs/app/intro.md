---
sidebar_position: 1
sidebar_label: App Intro
title: Generated App Intro
---

```bash
$ ./CSV_App app
$ cd app && ./setup.sh
# modify .env and start the server
$ ./app
```

- This generates a standalone app in `./app` directory
- Checkout the sample app [here](https://github.com/mainlycricket/CSV_App/blob/main/app)
- It is a standalone app, it runs on its own
- Remember to modify the .env file and rebuild the app before starting the server

---

- For each table, there are five API routes:

  - `POST /tableName` - insert a single row
  - `GET /tableName` - get all rows with filtering, sorting and pagination enabled
  - `GET /tableNameByPK` - get single row by primary key
  - `PUT /tableName` - update single row by primary key
  - `DELETE /tableName` - delete single row by primary key

- Each API response has following structure in JSON format:

  | Field   | Description                            |
  | ------- | -------------------------------------- |
  | success | boolean flag                           |
  | message | string message                         |
  | data    | object for GET routes, null for others |

- Checkout the sample Postman collection [here](https://documenter.getpostman.com/view/25403102/2sA3s7iUZc)
