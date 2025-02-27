---
title: "sys.sp_flush_commit_table_on_demand (Transact-SQL)"
description: "sys.sp_flush_commit_table_on_demand (Transact-SQL)"
author: JetterMcTedder
ms.author: bspendolini
ms.date: "10/20/2022"
ms.service: sql
ms.subservice: system-objects
ms.topic: "reference"
f1_keywords:
  - "sp_flush_commit_table_on_demand "
  - "sp_flush_commit_table_on_demand_TSQL"
  - "sys.sp_flush_commit_table_on_demand"
  - "sys.sp_flush_commit_table_on_demand_TSQL"
helpviewer_keywords:
  - "sys.sp_flush_commit_table_on_demand"
  - "sp_flush_commit_table_on_demand"
dev_langs:
  - "TSQL"
---
# sys.sp_flush_commit_table_on_demand (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Deletes rows from syscommittab in batches.
  
 :::image type="icon" source="../../includes/media/topic-link-icon.svg" border="false"::: [Transact-SQL syntax conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## Syntax

```sql
sys.sp_flush_commit_table_on_demand [ @numrows = ] numrows
[ , [@deleted_rows = ] deleted_rows OUTPUT ]
[ , [@date_cleanedup = ] date_cleanedup OUTPUT ]
[ , [@cleanup_ts = ] cleanup_ts OUTPUT ] 
```

## Arguments

'[@numrows = ] numrows' is the number of rows you want to delete from syscommittab. numrows is a bigint and cannot be NULL.

## Return Code Values

 **0** (success) or **1** (failure)

## Example

```sql
DECLARE @deleted_rows bigint;
DECLARE @date_cleanedup datetime;
DECLARE @cleanup_ts bigint;

exec sys.sp_flush_commit_table_on_demand 3000, @deleted_rows = @deleted_rows OUTPUT, 
    @date_cleanedup = @date_cleanedup OUTPUT, @cleanup_ts = @cleanup_ts OUTPUT;

print concat('Number of rows deleted: ', @deleted_rows);
print concat('Cleanup Date: ', @date_cleanedup);
print concat('Change Tracking Version: ', @cleanup_ts);
GO

Started executing query at Line 1
The value returned by change_tracking_hardened_cleanup_version() is 17.
The value returned by safe_cleanup_version() is 17.
(0 rows affected)
Number of rows deleted: 100
Cleanup Date: Aug 29 2022  8:59PM
Change Tracking Version: 17
Total execution time: 00:00:02.008
```

## Remarks

This procedure must be run in a database that has change tracking enabled.

## Permissions

 Only a member of the sysadmin server role or db_owner database role can execute this procedure.  
  
## See Also

 [About Change Tracking &#40;Transact-SQL&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md)  
 [Change Tracking Cleanup and Troubleshooting &#40;Transact-SQL&#41;](../../relational-databases/track-changes/cleanup-and-troubleshoot-change-tracking-sql-server.md)  
 [Change Tracking Functions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)  
 [Change Tracking System Tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/change-tracking-tables-transact-sql.md)  
 [Change Tracking Stored Procedures &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/change-tracking-stored-procedures-transact-sql.md)  
