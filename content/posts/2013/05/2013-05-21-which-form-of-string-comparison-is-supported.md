---
categories:
- Software Development
date: 2013-05-21 09:19:42+01:00
draft: false
tags:
- database
- sql
- Programming
- old blog
title: Which Form of String Comparison Is Supported
---
Below example is for Oracle DB, but adopting for other DBMS e.g. Sybase, MS SQL… should be straightforward.

```
select ‘both no space’ as “String comparison in Oracle” from dual where ‘a’ = ‘a’
--
union
select ‘right has single trailing space’ from dual where ‘a’ = ‘a ‘
union
select ‘right has multiple trailing space’ from dual where ‘a’ = ‘a   ‘
union
select ‘right has single leading space’ from dual where ‘a’ = ‘ a’
union
select ‘right has multiple leading space’ from dual where ‘a’ = ‘    a’
--
union
select ‘left has single trailing space’ from dual where ‘a ‘ = ‘a’
union
select ‘left has multiple trailing space’ from dual where ‘a   ‘ = ‘a’
union
select ‘left has single leading space’ from dual where ‘ a’ = ‘a’
union
select ‘left has multiple leading space’ from dual where ‘    a’ = ‘a’
--
union
select ‘right has single leading and trailing space’ from dual where ‘a’ = ‘ a ‘
union
select ‘right has multiple leading and trailing space’ from dual where ‘a’ = ‘    a   ‘
union
select ‘left has single leading and trailing space’ from dual where ‘ a ‘ = ‘a’
union
select ‘left has multiple leading and trailing space’ from dual where ‘   a      ‘ = ‘a’
—-
union
select ‘both has leading and trailing space but leading space are different’ from dual where ‘ a ‘ = ‘  a ‘
```
