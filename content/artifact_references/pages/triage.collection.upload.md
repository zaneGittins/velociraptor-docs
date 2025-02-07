---
title: Triage.Collection.Upload
hidden: true
tags: [Client Artifact]
---

A Generic uploader used by triaging artifacts.


```yaml
name: Triage.Collection.Upload
description: |
  A Generic uploader used by triaging artifacts.

parameters:
  - name: path
    description: This is the glob of the files we use.
  - name: type
    description: The type of files these are.
  - name: accessor
    default: file

sources:
  - query: |
        LET results = SELECT OSPath, Size,
               Mtime As Modifed,
               type AS Type,
               upload(file=OSPath,
                      accessor=accessor,
                      ctime=Ctime,
                      mtime=Mtime) AS FileDetails
        FROM glob(globs=path, accessor=accessor)
        WHERE NOT IsDir

        SELECT OSPath, Size, Modifed, Type,
               FileDetails.Path AS ZipPath,
               FileDetails.Md5 as Md5,
               FileDetails.Sha256 as SHA256
        FROM results

```
