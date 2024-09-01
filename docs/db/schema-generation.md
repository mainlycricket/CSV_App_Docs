---
sidebar_position: 1
sidebar_label: Schema Generation
title: Schema Generation
---

```bash
$ ./CSV_App schema
```

- This generates an initial schema in `data/schema.json` by analyzing the CSV files
- Constraints in `data/schema.json` should be reviewed
- [Special constraints](./db-constraints.md) like `Min`, `Max`, `Enums`, `Default` & `Hash` are always required to be set manually
- Checkout the sample schema file [here](https://github.com/mainlycricket/CSV_App/blob/main/data/schema.json)
- No data validation is performed

---
- Always successful unless:
  - an error is encountered while parsing `.csv` files
  - duplicate table name or column name name is found
  - empty table name or column name is found

- Table Names & Column Names should NOT be changed as they are **_sanitized_**

:::danger[Label Sanitization]
- Leading & Trailing spaces are trimmed.
- Any sequence of non-alphanumeric character is replaced by a single underscore.
- E.g. `!'Table Name'!` is transformed into `_Table_Name_`
- Hence, two tables or their columns can't have the same sanitized name
:::