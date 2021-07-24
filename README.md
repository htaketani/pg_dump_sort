pg_dump_sort
============

Sort table rows data by primary key in pg_dump output sql.
This is usefull for diffing database structure and contents.

## Description

PostgreSQL's pg_dump command can output database structure and contents,
but contents is not sorted.
This script sort dumped file by table primary key definition.

```sh
# pg_dump or pg_dumpall (not use `--inserts` option)
$ pg_dump somedb > dump.sql

# sort rows data by primary key
$ pg_dump_sort dump.sql > dump-sorted.sql
```

## Requirements

* perl 5.6 or later

## Installation

Installation is not needed.
Download this script, and move to PATH directory, and set execution-bit by chmod command.

## Supported PostgreSQL(pg_dump) versions, dumpfile format

* (maybe) All PostgreSQL versions
    * Checked versions: v11.3, v11.5
* pg_dump's default output format
    * Used `COPY` for importing table rows data
    * Not supported pg_dump `--inserts`(`-d`) option

## Usage

```
Usage:
  pg_dump_sort [DUMP_FILE]
```

* Output sorted dump to STDOUT.
* Table rows data are sorted by primary key columns of it's table.
    * If the table has no primary key, used first unique key constraint columns instead of primary key.
    * Functional index is not supported.
        * e.g. `CREATE INDEX user_name_idx ON user (lower(firstname), lower(lastname));`

## Known issues

* PostgreSQL can use quoted any special chars as identifier.
    * e.g. Next quoted identifier is valid syntax. can use as table name, column name, etc.
        ```
        "spaces comma, round-bracket( single-quote' double-quote"" linefeed

        etc..."
        ```
    * This script supports dobule-quoted identifier.
    * But this script parse easy, not stricted.
    * If table or column includes `(`, `, `, and linefeed, then It may not work well.

## Thanks

This script is inspired by [tigra564/pgdump-sort](https://github.com/tigra564/pgdump-sort),
thank you, tigra564

