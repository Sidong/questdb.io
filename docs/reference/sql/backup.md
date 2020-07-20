---
title: BACKUP
sidebar_label: BACKUP
---

## Syntax

![backup syntax](/img/doc/diagrams/backup.svg)

## Description

Creates a backup for one, several, or all database tables.

## Backup Directory

:::tip
`BACKUP TABLE` requires a `backup directory` which is set using the
[configuration key](reference/server-configuration.md) `cairo.sql.backup.root` in the
[server.conf](reference/root-directory-structure.md#serverconf) file.
:::

```script title="Example configuration key"
cairo.sql.backup.root=/Users/UserName/Desktop
```

The `backup directory` can be on a local disk to the server, on a remote disk,
or a remote filesystem. QuestDB will enforce that the backup are only written in
a location relative to the `backup directory`. This is a security feature to
disallow random file access by QuestDB.

The tables will be written in a directory with today's date. By default, the
format is `yyyy-MM-dd`, for example `2020-04-20`.

:::tip
You can define a custom format using the
`cairo.sql.backup.dir.datetime.format` [configuration key](reference/server-configuration.md) like
the example below
:::

```script title="Example user-defined directory format"
cairo.sql.backup.dir.datetime.format=yyyy-dd-MM
```

The data and meta files will be written following the
[db directory structure](reference/root-directory-structure.md#db)

```filestructure title="Directory structure (single backup)"
'backup directory/'
2020-04-20
├── table1
├── table2
└── ...
```

If a user performs several backups on the same date, each backup will be written
a new directory. Subsequent backups on the same date will look as follows:

```filestructure title="Directory structure (multiple backups)"
'backup directory/'
├── 2020-04-20      'first'
├── 2020-04-20.1    'second'
├── 2020-04-20.2    'third'
├── 2020-04-21      'first new date'
├── 2020-04-21.1    'first new date'
└── ...
```

## Examples

```questdb-sql title="Single table"
BACKUP TABLE table1;
```

```questdb-sql title="Multiple tables"
BACKUP TABLE table1, table2, table3;
```

```questdb-sql title="All tables"
BACKUP DATABASE;
```