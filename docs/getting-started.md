---
sidebar_position: 1
sidebar_label: Getting Started
title: Getting Started
---

- Clone the [project](https://github.com/mainlycricket/CSV_App)
- Delete the existing `./app` directory
- Remove the files in `./data` directory and copy your CSV files here.

```bash
git clone git@github.com:mainlycricket/CSV_App.git
cd CSV_App
rm -r app
rm data/*
# copy csv files in ./data
```

- `data/schema.json` is used for db.sql generation
- `data/schema.json` and `data/appConfig.json` are used for app generation
