---
sidebar_position: 5
sidebar_label: Tables Config
title: Tables Config
---

- Configuration is set for each table individually in `tables` object of `data/appConfig.json`
- There are following config options for each table: `readAllConfig`, `readByPkConfig`, `defaultPagination`, and auth configs: `insertAuth`, `readAllAuth`, `readByPkAuth`, `updateAuth`, `deleteAuth`.
- Explained with an example of the `students` table configuration

### Read Configs

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

- `readAllConfig` and `readByPkConfig` are used for the GET all and GET by primary key requests respectively
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

### Auth Configs

- Auth Configs are configurable for each kind of request: GET, POST, PUT and DELETE
- Each auth config contains following fields: `userField`, `orgFields`, `basicAuth`, `allowedRoles`, `privileges` and `protectedFields`
- These fields are explained below.

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

### Basic Auth & Allowed Roles

<details>
  <summary>Sample Basic Auth & Allowed Roles</summary>

    ```json
    {
      "basicAuth": true,
      "allowedRoles": ["admin", "principal"]
    }
    ```

</details>

- The `basicAuth` flag, if set, ensures that only logged in users can access the particular resource
- The `allowedRoles` restricts the resource to users with the mentioned roles. It can be set only if the `basicAuth` flag is enabled

### Privileges & Protected Fields

<details>
  <summary>Sample Privileges & Protected Fields</summary>

    ```json
    {
      "privileges": {
        "Course_Id": ["principal"],
        "Branch_Id": ["principal"]
      },
      "protectedFields": {
        "verified": {
          "true": ["principal"]
        }
      }
    }
    ```

    - Here users with `principal` roles should explicitly set the value for the `Course_Id` and `Branch_Id` (org) fields
    - The `true` value for the `verified` field can be set by users with the `principal` role

</details>

- If you want users with some particular roles to explicit set the value of `userField` or `orgFields`, use `privileges` to set the config
- For enum-based or boolean columns, you can restrict the users who can set some particular values by using the `protectedFields` object. Its structure is as follows:

  ```json
  {
    "protectedFields": {
      "field1": {
        "value1": ["role1", "role2"],
        "value2": ["role1"]
      },
      "field2": {
        "value1": ["role1"],
        "value2": ["role1", "role2"]
      }
    }
  }
  ```

  :::tip
  The values should be mentioned in strings in the same format as the CSV files
  :::

- It is possible to use `privileges` and `protectedFields` without enabling `basicAuth`. Mention the normal roles as you would, and to address the non-logged-in users, use empty strings `""` in the roles array

  ```json
  {
    "privileges": {
      "orgField1": ["role1", ""]
    },
    "protectedFields": {
      "field1": {
        "value1": ["role1", ""]
      }
    }
  }
  ```

### Insert Auth

<details>
  <summary>Sample Insert Auth</summary>

    ```json
    {
      "insertAuth": {
        "userField": "added_by",
        "orgFields": ["college_id", "Course_Id", "Branch_Id"],
        "basicAuth": true,
        "allowedRoles": ["principal", "HoD"],
        "privileges": {
          "Course_Id": ["principal"],
          "Branch_Id": ["principal"]
        },
        "protectedFields": {
          "verified": {
            "true": ["principal"]
          }
        }
      }
    }
    ```

    - Values for the `added_by` and `college_id` fields will be set according to the JWT token for both `principal` & `HoD` users
    - Values for `Course_Id` and `Branch_Id` are required to be set explicitly by users with the `principal` role (and set according to token for `HoD` users)
    - Also, only `principal` users can set `true` value for the `verified` field

</details>

- The `insertAuth` is used to control the behaviour of the `POST` requests
- This config, except `basicAuth` and `allowedRoles`, is also used in `PUT` requests to handle the `SET` clause.

### Read Auth

<details>
  <summary>Sample Read All Auth</summary>

    ```json
    {
      "readAllAuth": {
        "orgFields": ["college_id", "Course_Id", "Branch_Id"],
        "basicAuth": true,
        "allowedRoles": ["principal", "HoD", "teacher"],
        "privileges": {
          "Course_Id": ["principal"],
          "Branch_Id": ["principal"]
        },
        "protectedFields": {
          "verified": {
            "false": ["principal"]
          }
        }
      }
    }
    ```

    - Value for the `college_id` field will be set according to the JWT token for all of `principal`, `HoD` & `teacher` users
    - Values for `Course_Id` and `Branch_Id` can be set explicitly by users with the `principal` role (and set according to token for `HoD` & `teacher` users)
    - Also, only `principal` users can set `false` value for the `verified` field, meaning only they can access unverified students

</details>

- The `readAllAuth` and `readByPkAuth` are used to control the behaviour of the `GET` all and `GET` by primary key requests respectively

### Update Auth

<details>
  <summary>Sample Update Auth</summary>

    ```json
    {
      "deleteAuth": {
        "orgFields": ["college_id", "Course_Id", "Branch_Id"],
        "basicAuth": true,
        "allowedRoles": ["principal", "HoD"],
        "privileges": {
          "Course_Id": ["principal"],
          "Branch_Id": ["principal"]
        }
      }
    }
    ```

    - Value for the `college_id` field will be set according to the JWT token for all of `principal` & `HoD` users
    - Values for `Course_Id` and `Branch_Id` can be set explicitly by users with the `principal` role (and set according to token for `HoD` users)

</details>

- The `updateAuth` is used to control the `WHERE` clause in `PUT` requests
- In the given example, users with the `principal` role can update any student of his college, but HoDs can update students only of his department
- Just to reiterate, the `insertAuth` config applies in the `SET` clause here. For instance, users with only `principal` role can set the `true` value for the `verified` field as mentioned in the `insertAuth`

### Delete Auth

<details>
  <summary>Sample Delete Auth</summary>

    ```json
    {
      "deleteAuth": {
        "orgFields": ["college_id", "Course_Id", "Branch_Id"],
        "basicAuth": true,
        "allowedRoles": ["principal", "HoD"],
        "privileges": {
          "Course_Id": ["principal"],
          "Branch_Id": ["principal"]
        }
      }
    }
    ```

    - Value for the `college_id` field will be set according to the JWT token for all of `principal` & `HoD` users
    - Values for `Course_Id` and `Branch_Id` can be set explicitly by users with the `principal` role (and set according to token for `HoD` users)

</details>

- The `deleteAuth` is used to control the `WHERE` clause in `DELETE` requests
- In the given example, users with the `principal` role can delete any student of his college, but HoDs can delete students only of his department
