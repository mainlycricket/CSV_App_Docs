---
sidebar_position: 3
sidebar_label: GET Query Options
title: GET Query Options
---

GET all requests can add various query params to customize the response

<details>
<summary>Sample students GET all response</summary>

```json
{
  "success": true,
  "message": "data fetched successfully",
  "data": {
    "next": false,
    "data": [
      {
        "Branch_Id": {
          "Branch_Id": "branch_1",
          "Branch_Name": "Computer Science"
        },
        "Course_Id": {
          "Course_Id": "course_1",
          "Course_Name": "B. Tech."
        },
        "Student_Father": "Ajay",
        "Student_Id": 1,
        "Student_Name": "Tushar",
        "college_id": {
          "college_id": "college_1",
          "college_name": "IIT Delhi"
        }
      },
      {
        "Branch_Id": {
          "Branch_Id": "branch_1",
          "Branch_Name": "Computer Science"
        },
        "Course_Id": {
          "Course_Id": "course_1",
          "Course_Name": "B. Tech."
        },
        "Student_Father": "Nand",
        "Student_Id": 2,
        "Student_Name": "Akshay",
        "college_id": {
          "college_id": "college_1",
          "college_name": "IIT Delhi"
        }
      }
    ]
  }
}
```

- The `data` object contains two major fields: `next` indicates where more data is avaiable - `data` contains the actual data
</details>

### Filters

- For data filtering, the keys should have the same name as column names in schema

- For each column, multiple values can be passed by separating them with a comma

  ```
  ?standard=5,6

  This filters all the rows which have a value of `5` or `6` for the `standard` column
  ```

- Multiple columns are separated by an ampersand (&)

  ```
  ?standard=5,6&date=2024-01-15

  This filters all the rows which have a value of `5` or `6` for the `standard` column and `2024-01-15` value for the `date` column
  ```

- For array fields, if the array contains any of the passed values, the condition is true

  ```
  ?active_on=2024-02-02,2024-06-02

  This filters all the rows in which the `active_on` array contains `2024-02-02` or `2024-06-02` items
  ```

- For string fields including string array fields, values should be passed in separate pairs. This is needed because string fields may contain special symbols

  ```
  ?name=Tushar&name=Akshay

  This filters all the rows which have a value of `Tushar` or `Akshay` for the `name` column
  ```

  ```
  ?colors=green&colors=red

  This filters all the rows in the `colors` array contains the `green` or `red` items
  ```

:::tip
[Encode the URL](https://www.freecodecamp.org/news/javascript-url-encode-example-how-to-use-encodeuricomponent-and-encodeuri/) after adding the query params
:::

### Sorting

- Sorting can be customized using `__order` query param
- Data is by-default sorted using the primary key field if the `__order` param is not specified
- Data can be sorted based on multiple fields, which should be separated by a comma
- Prefix the column name with a hyphen to sort data in the descending order
- For foreign key columns, sorting by columns of the foreign table is not supported

```
?__order=productName,-price

- This sorts data by `productName` in ascending order and then by `price` in descending order
```

### Pagination

- `__page` param to specify the page number, by default 1
- `__limit` param to specify the maximum number of records, by default specified in [tables config](./tables-config#pagination)

```
?__order=studentName&__page=2&__limit=25

- This sorts data by `studentName` in ascending order
- Skips the first 25 records, and returns the next 25 records (at most)
```
