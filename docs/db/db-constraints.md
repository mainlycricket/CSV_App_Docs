---
sidebar_position: 2
sidebar_label: DB Constraints
title: DB Constraints
---

Details about the constraints set in `data/schema.json` :

### Data Types

- integer
- float
- boolean
- text
- date
- time
- datetime (with timezone)
- array of these primitive types

:::tip
Checkout [TypeTest.csv](https://github.com/mainlycricket/CSV_App/blob/main/data/TypeTest.csv) to understand data formats in CSV files
:::

:::danger[Note]
If all the values of any column are empty, its datatype is marked as empty and should be set manually in schema.json
:::

### Special Constraints

| Code | Constraint  |
| ---- | ----------- |
| N    | Not Null    |
| U    | Unique      |
| P    | Primary Key |
| F    | Foreign Key |

- These constraint codes can be mentioned before the column name in the CSV files
- Constraint code & column name should be separated by a colon (:)
- Presence of 'P' overrides all the other constraints, P also sets the Not Null & Unique
- Extra unwanted characters in constraints are ignored
- Eg: `P:columnName`, `U:columnName`, `NU:columnName`, `FN:columnName`, `FNU:columnName`

:::danger[Note]
If a colon is present in a column label, the left part will be treated as constraint
:::

### Primary Key

- Composite Primary Keys are not allowed
- If no primary key is provided, a column named `__ID` is added by default in SQL file and the generated app but `schema.json`remains unmodified
- All datatypes are allowed to be primary key

### Foreign Key Mapping

If a column is marked as a foreign key, the referenced column is mapped with the following idea:

- if any other table has a column with the same name and datatype, it is marked as referenced column
- if multiple such columns exist, any one is choosen
- if no match is found, the `ForeignTable` & `ForeignColumn` fields are left with value `__`

### Min & Max

- Should always be mentioned in strings
- Should be empty for boolean values or if they aren't required
- Data is validated by value for integer, float, date, time, datetime
- E.g. `Min:"3"` and `Max:"10"` for `integer` mean `3 >= value <= 10`
- Data is validated by length for strings (should be a +ve integer)
- E.g. `Min:"3"` and `Max:"10"` for `text` mean `3 >= length(value) <= 10`
- For array: "array_length,individual_value_constraint"
- E.g. `Min: "2,3"` and `Max: "5,10"` for `integer[]` implies `2 >= len(arr) <= 5` and `3 >= each_element <= 10`
- E.g. `Min: "2,3"` and `Max: "4,10"` for `text[]` implies `2 >= len(arr) <= 4` and `3 >= length(each_element) <= 10`

### Enums

- Enums should be an array that specifies the allowed values for the column
- Each indiviual value in Enums should satisfy the individual `Min` and `Max` constraints
- For array columns, each values_arr[i] should be present in enums array

### Default

- Default value should satisy the min, max and enums constraint if they're present
- Primary Key & Unique columns shouldn't have a default value

### Hash

- `text` and `text[]` columns are be hashed if the `Hash` constraint is enabled.
