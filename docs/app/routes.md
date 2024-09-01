---
sidebar_position: 2
sidebar_label: API Routes
title: API Routes
---

### POST /tableName

- The request body should have JSON data
- The value for fallback primary key `__ID`, in case of no primary key in table schema, will be added automatically by PostgreSQL
- Data is validated against constraint by PostgreSQL, not at the application level as of now

### GET /tableName

- All the table data is returned in JSON format
- Foreign Key columns data is looked-up from the referenced table
- Data filtering, sorting and pagination is supported by query params
- Read more [here](./get-req.md)

### GET /tableNameByPK

- It selects a single row based on the primary key received as query param `?id=value`
- The key should always be `id` regardless of the primary key column name in the table
- For array columns except string arrays, send values as `localhost:8080/tableByPK?id=1,2,3`
- For string columns, send values as individual string elements as `localhost:8080/tableByPK?id=text1&id=text2&id=text3`

### PUT /tableName

- It updates a single row based on the primary key received as query param `?id=value`
- Query param should be passed in the same manner as `GET /tableNameByPK` route
- The request body should be the same as the POST request but the fallback primary key `__ID` is required even if it is not modified

### DELETE /tableName

- It deletes a single row based on the primary key received as query param `?id=value`
- Query param should be passed in the same manner as `GET /tableNameByPK` route
