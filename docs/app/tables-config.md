---
sidebar_position: 4
sidebar_label: Tables Config
title: Tables Config
---

- Configuration is set for each table individually in `tables` object of `data/appConfig.json`
- Explained with an example of the `students` table configuration

### User & Org Fields

<details>
  <summary>Sample User & Org Fields</summary>

    ```json
    {
      "userField": "added_by",
      "orgFields": {
        "college_id": "college_id",
        "Course_Id": "course_id",
        "Branch_Id": "branch_id"
      },
    }
    ```

    - Here, the value for the `added_by` column will be taken from the `username` in JWT token
    - For `Course_Id`, the value will be taken from the `course_id` in JWT token
    - Similary for `college_id` and `Branch_Id`

    :::tip
    You may want to set the `college_id`, `Course_Id`, `Branch_Id` as foreign keys to the respective `colleges`, `courses`, `branches` table
    :::

</details>

- These are the fields whose value can be taken from the JWT `access_token` cookie, in all kinds of request - POST, GET, PUT, DELETE
- The key in the `orgFields` is the column name in the table and the value is the corresponding column name in the _auth_ table
- The value for the `userField` column will be taken from `username` field from JWT token
- The value for the `orgFields` column will be taken from the corresponding field from JWT token

### Read & Write Auth

<details>
  <summary>Sample Read & Write Auth</summary>

```json
{
  "readAuth": {
    "basicAuth": true,
    "allowedRoles": ["principal", "hod"],
    "priviliges": {
      "Course_Id": ["principal"],
      "Branch_Id": ["principal"]
    }
  },
  "writeAuth": {
    "basicAuth": true,
    "allowedRoles": ["principal", "hod"],
    "priviliges": {
      "Course_Id": ["principal"],
      "Branch_Id": ["principal"]
    }
  }
}
```

- Basic Auth is enabled to filter only the logged-in users
- Further, only `principal` and `hod` users are allowed to read from or write to students table
- `principal` users should set the `Course_Id` and `Branch_Id` explicitly
- To put it clearly, the `principal` users can add students to any course & branch while the `hod` users can only add students to their course and branch. Similarly for the read operation

</details>

- `readAuth` works for GET requests
- `writeAuth` works for POST, PUT & DELETE requests
- The `basicAuth` flag can be set to enable basic authentication i.e. user must be logged in
- The `allowedRoles` array should contain roles which are authorized to access the resource
- In case you want that some users can explicitly set the values of `userField` or `orgFields` columns, use the `priviliges` object
- The key should be either the `userField` or one of the `orgFields`, and the value are the user roles who can explicit the values for these columns

:::danger[NOTE]
basicAuth must be set if either of allowedRoles or priviliges is enabled
:::

### Read Config

<details>

  <summary>Sample Read All Config</summary>

```json
{
  "readAllConfig": {
    "columns": [
      "Branch_Id",
      "Student_Id",
      "Student_Name",
      "Course_Id",
      "Student_Father",
      "college_id"
    ],
    "foreignColumns": {
      "Branch_Id": ["Branch_Id", "Branch_Name"],
      "Course_Id": ["Course_Id", "Course_Name"],
      "college_id": ["college_id", "college_name"]
    }
  }
}
```

- The `columns` array specifies the fields to be selected
- For the `Branch_Id` foreign key, the `Branch_Id` and `Branch_Name` columns should be looked up
- Similarly for the `Course_Id` and `college_id`

</details>

- `readAllConfig` is used for the GET all requests while `readByPkConfig` is used for GET by primary key requests
- The `columns` array specifies the fields to be selected from the table
- The `foreignColumns` specifies the fields which should be looked up from the foreign table
- If foreign table look-up is not required for a particular field, it need not be there in the `foreignColumns`
- Hashed fields can't be included

:::danger[note]
If a field is present in `foreignColumns`, it must be present in the `columns` to mark it as selected in the first place

:::

### Pagination

<details>

  <summary>Sample Pagination</summary>

```json
{
  "defaultPagination": 20
}
```

- If no `__limit` query param is provided, at most 20 records will be returned by default

</details>

- Used to set the default number of records to be returned in GET _all_ requests if the `__limit` param is not specified
- It must be a non-zero positive number, hence pagination is not optional
