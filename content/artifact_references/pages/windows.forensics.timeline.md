---
title: Windows.Forensics.Timeline
hidden: true
tags: [Client Artifact]
---

Win10 records recently used applications and files in a “timeline”
accessible via the “WIN+TAB” key. The data is recorded in a SQLite
database.

## NOTES:

This artifact is deprecated in favor of
Generic.Forensic.SQLiteHunter and will be removed in future


```yaml
name: Windows.Forensics.Timeline
description: |
  Win10 records recently used applications and files in a “timeline”
  accessible via the “WIN+TAB” key. The data is recorded in a SQLite
  database.

  ## NOTES:

  This artifact is deprecated in favor of
  Generic.Forensic.SQLiteHunter and will be removed in future

parameters:
  - name: UserFilter
    default: ""
    description: If specified we filter by this user ID.
    type: regex

  - name: ExecutionTimeAfter
    default: ""
    type: timestamp
    description: If specified only show executions after this time.

  - name: Win10TimelineGlob
    default: C:\Users\*\AppData\Local\ConnectedDevicesPlatform\*\ActivitiesCache.db

precondition: SELECT OS From info() where OS = 'windows'

sources:
  - query: |
      LET timeline = SELECT * FROM foreach(
         row={
            SELECT OSPath
            FROM glob(globs=Win10TimelineGlob)
         },
         query={
            SELECT AppId, OSPath, LastModifiedTime
            FROM sqlite(file=OSPath, query="SELECT * FROM Activity")
         })

      LET TMP = SELECT get(
      item=parse_json_array(data=AppId).application,
               member="0") AS Application,
             parse_string_with_regex(
               string=OSPath,
               regex="\\\\L.(?P<User>[^\\\\]+)\\\\").User AS User,
               LastModifiedTime,
               LastModifiedTime.Unix as LastExecutionTS
        FROM timeline

      LET A1 = SELECT * FROM if(
          condition=UserFilter,
          then={
            SELECT * FROM TMP WHERE User =~ UserFilter
          }, else={ SELECT * FROM TMP})

      SELECT * FROM if(
          condition=ExecutionTimeAfter,
          then={
            SELECT * FROM A1 WHERE LastExecutionTS > ExecutionTimeAfter
          }, else={ SELECT * FROM A1})

```
