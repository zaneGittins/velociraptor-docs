---
title: Windows.Applications.Firefox.History
hidden: true
tags: [Client Artifact]
---

Enumerate the users Firefox history.

## NOTES:

This artifact is deprecated in favor of
Generic.Forensic.SQLiteHunter and will be removed in future


```yaml
name: Windows.Applications.Firefox.History
description: |
  Enumerate the users Firefox history.

  ## NOTES:

  This artifact is deprecated in favor of
  Generic.Forensic.SQLiteHunter and will be removed in future


author: Zach Stanford @svch0st, Modified by @angry-bender
parameters:
  - name: placesGlobs
    default: \AppData\Roaming\Mozilla\Firefox\Profiles\*\places.sqlite
  - name: urlSQLQuery
    default: |
        SELECT *,url as url_visited FROM moz_historyvisits, moz_places WHERE moz_historyvisits.place_id=moz_places.id
  - name: userRegex
    default: .
    type: regex
  - name: URLRegex
    default: .
    type: regex

precondition: SELECT OS From info() where OS = 'windows'

sources:
  - query: |
        LET places_files = SELECT * from foreach(
          row={
             SELECT Uid, Name AS User,
                    expand(path=Directory) AS HomeDirectory
             FROM Artifact.Windows.Sys.Users()
             WHERE Name =~ userRegex
          },
          query={
             SELECT User, OSPath, Mtime
             FROM glob(root=HomeDirectory, globs=placesGlobs)
          })

        SELECT * FROM foreach(row=places_files,
          query={
            SELECT User, OSPath,
                   timestamp(epoch=visit_date/1000000) as visit_time,
                   place_id,url_visited,title,rev_host,visit_count,hidden,typed,description
            FROM sqlite(
              file=OSPath,
              query=urlSQLQuery)
          })
          WHERE url_visited =~ URLRegex

```
