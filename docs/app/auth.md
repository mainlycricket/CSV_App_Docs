---
sidebar_position: 4
sidebar_label: Auth
title: Auth
---

### Enabling Auth

- To enable auth in the application, a dedicated CSV file should be used
- It is recommended to keep only auth related info in this table
- This table must have:
  - `username` column of `text` type as its primary key
  - `password` column of `text` type with `Hash` enabled
- This table may also have optional fields to enable role/organization based authorization:
  - `role` field of `text` type with `Enums` set
  - any number of _organizational_ fields - column names can be user-decided but they must be of `text` type
- Checkout the sample `login.csv` [here](https://github.com/mainlycricket/CSV_App/blob/main/data/login.csv)
- The `authTable` field in `data/appConfig.json` should be the name of auth table from `schema.json` which satisfies the above mentioned constraints
- The `orgFields` array should contain the column names choosen as _organizational_ fields in the `authTable`

:::danger[Note]
In auth CSV table, the username, password and role fields must be named as it is
:::

:::tip
The organizational fields can be used to identify organzation, sub-organization, departments, sub-departments and so on
:::

### Register Route

- Send POST request to `/__auth/register` with the JSON request body

<details>
  <summary>Sample Register Request Body</summary>

```json
{
  "username": "john_doe",
  "password": "secret",
  "role": "hod",
  "college_id": "college_1",
  "course_id": "course_1",
  "branch_id": "branch_1"
}
```

</details>

### Login Route

- Send POST request to `/__auth/login` with JSON request body

<details>
  <summary>Sample Login Request Body</summary>

```json
{
  "username": "john_doe",
  "password": "secret"
}
```

</details>

- A JWT cookie named `access_token` with an expiry date of `30 days` is set to the response object, which is automatically sent along with the future requests

- The JWT token contains user info like `username`, `role` etc.

<details>
  <summary>Sample JWT token info</summary>

```json
{
  "username": "john_doe",
  "role": "hod",
  "college_id": "college_1",
  "course_id": "course_1",
  "branch_id": "branch_1"
}
```

</details>

### Logout Route

- Send GET request to `/__auth/logout`

### Refresh Token Route

- Send GET request to `/__auth/refresh`
- Can be used to verify the current access token in the cookie
- Sets an updated access token in the cookie
