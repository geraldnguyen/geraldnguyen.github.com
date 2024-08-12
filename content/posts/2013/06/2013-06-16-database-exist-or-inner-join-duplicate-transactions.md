---
title: "[Database] Exists or Inner Join: Duplicate Records"
date: 2013-06-16T09:19:42+01:00
draft: false
weight: 50
tags: [database, sql, programming, old blog]
---

## INNER JOIN vs. EXISTS

Often, EXISTS and INNER JOIN can be used interchangeably to query data with existence in 2 or more tables. However, it is not always the case. There is an important inherent characteristics of INNER JOIN that every developer should keep in mind: INNER JOIN result may contain DUPLICATE data record. This is important for uniqueness-sensitive handling of query result (e.g. record count), or when migrating database table with unique constraints.

EXISTS offers a better uniqueness guarantee in such situation. EXISTS will maintain uniqueness of each record from original `TABLE_NAME` when copying to new `COPY_TABLE_NAME`. In other words, if a particular record satisfying query condition has a single record in `TABLE_NAME`, it will have a single presence in `COPY_TABLE_NAME`.

## An example

We want to copy data from `TABLE_NAME` to `COPY_TABLE_NAME` with the help of a look up table `LOOKUP_TABLE`, perform some conversion, verification, etc on `COPY_TABLE_NAME`, then truncate `TABLE_NAME` and copy data from `COPY_TABLE_NAME` back to original `TABLE_NAME`. (The reason for operating on COPY_TABLE_NAME and copying back to TABLE_NAME is to avoid corrupting original data from unexpected error during data conversion). Our focus in this post is in extracting data from `TABLE_NAME` to `COPY_TABLE_NAME`.

Assume all 3 tables share a same-name `PK_COL` column.

### Scenario 1: When INNER JOIN and EXISTS return same result – when look-up table has unique value on join column

Assume the following condition is used to extract data: Select from `TABLE_NAME` every record whose value in `PK_COL` is also present in `LOOKUP_TABLE.PK_COL`. Both `TABLE_NAME` and `LOOKUP_TABLE` have unique constrain on `PK_COL`. In this case, EXISTS and INNER JOIN is interchangeable:

```

INSERT INTO COPY_TABLE_NAME
(    SELECT B.*
FROM TABLE_NAME B
WHERE EXISTS (SELECT 1 FROM LOOKUP_TABLE c WHERE c.PK_COL = B.PK_COL)
);

INSERT INTO COPY_TABLE_NAME
(    SELECT B.*
FROM TABLE_NAME B, LOOKUP_TABLE c
WHERE c.PK_COL = B.PK_COL
);
```

### Scenario 2: When INNER JOIN returns duplicates while EXISTS preserve uniqueness of source table

However, if `LOOKUP_TABLE` has no unique constraint on `PK_COL`, INNER JOIN can result in duplicate record.

Or, if extraction condition contains an OR, like the below example, INNER JOIN can result in duplication even if LOOKUP_TABLE has unique constraint,

INNER JOIN

```
INSERT INTO COPY_TABLE_NAME
(    SELECT B.*
FROM TABLE_NAME B, LOOKUP_TABLE c
WHERE c.PK_COL = B.PK_COL OR c.PK_COL = B.ANOTHER_COL
);
```

In this case, EXISTS offers a better guarantee for data integrity and uniqueness.

EXISTS

```
INSERT INTO COPY_TABLE_NAME
(    SELECT B.*
FROM TABLE_NAME B
WHERE EXISTS (SELECT 1 FROM LOOKUP_TABLE c WHERE c.PK_COL = B.PK_COL OR c.PK_COL = B.ANOTHER_COL)
);
```

## Another Example

Let’s look at another example to understand this subtle difference between INNER JOIN and EXISTS. Assume we have 2 tables `TestTable1` and `TestTable2`. We will interchange these 2 tables for source and look-up table to illustrate 2 scenarios:

– When INNER JOIN and EXISTS return same result – when look-up table has unique value on join column
– When INNER JOIN returns duplicates while EXISTS preserve uniqueness of source table – when look-up tables has duplicate values in join column

```
--sample tables
create table TestTable1 (
    key1    varchar2(50),
    key2    varchar2(50)
)
;
commit;
 
create table TestTable2(
    key1    varchar2(50),
    key2    varchar2(50)
)
;
commit;
 
truncate table TestTable1;
truncate table TestTable2;
 
-- data: A, A, B, C
insert into TestTable1 values ('A', 'table1 - 1');
insert into TestTable1 values ('A', 'table1 - 2');
insert into TestTable1 values ('B', 'table1 - 1');
insert into TestTable1 values ('C', 'table1 - 1');
 
--A, X, Y, Z
insert into TestTable2 values ('A', 'table2 - i');
insert into TestTable2 values ('X', 'table2 - i');
insert into TestTable2 values ('Y', 'table2 - i');
insert into TestTable2 values ('Z', 'table2 - i');
```

### Scenario 1: When INNER JOIN and EXISTS return same result – when look-up table has unique value on join column

```
--select from Table1: 2 distinct records
select a.*
from TestTable1 a, TestTable2 b
where a.key1 = b.key1
;
 
--exists: table1 - 2 distinct records
select a.*
from TestTable1 a
where exists (select 1 from TestTable2 b where a.key1 = b.key1);
```

### Scenario 2: When INNER JOIN returns duplicates while EXISTS preserve uniqueness of source table – when look-up tables has duplicate values in join column

```
--select from Table2: 2 identical records
select b.*
from TestTable1 a, TestTable2 b
where a.key1 = b.key1
;
 
--exists: table2 - 1 record ONLY
select b.*
from TestTable2 b
where exists (select 1 from TestTable1 a where a.key1 = b.key1);
```