---
title: MacOS.Applications.Chrome.History
hidden: true
tags: [Client Artifact]
---

Read all User's chrome history.

## NOTES:

This artifact is deprecated in favor of
Generic.Forensic.SQLiteHunter and will be removed in future


```yaml
name: MacOS.Applications.Chrome.History
description: |
  Read all User's chrome history.

  ## NOTES:

  This artifact is deprecated in favor of
  Generic.Forensic.SQLiteHunter and will be removed in future


parameters:
  - name: historyGlobs
    default: /Users/*/Library/Application Support/Google/Chrome/*/History
  - name: urlSQLQuery
    default: |
      SELECT url as visited_url, title, visit_count,
             typed_count, last_visit_time
      FROM urls
  - name: userRegex
    default: .

precondition: SELECT OS From info() where OS = 'darwin'

sources:
  - query: |
      LET history_files = SELECT
         parse_string_with_regex(regex="/Users/(?P<User>[^/]+)", string=OSPath).User AS User,
         OSPath, Mtime
      FROM glob(globs=historyGlobs)

      SELECT * FROM foreach(row=history_files,
        query={
           SELECT User, OSPath,
              Mtime,
              visited_url,
              title, visit_count, typed_count,
              timestamp(winfiletime=last_visit_time * 10) as last_visit_time
          FROM sqlite(
             file=OSPath,
             query=urlSQLQuery)
          })

```
