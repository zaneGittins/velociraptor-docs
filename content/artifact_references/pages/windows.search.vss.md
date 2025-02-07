---
title: Windows.Search.VSS
hidden: true
tags: [Client Artifact]
---

This artifact will find all relevant files in the VSS. Typically
used to out deduplicated paths for processing by other artifacts.

NOTE: This used to be more complicated but now delegates to the
"ntfs_vss" accessor to do all the work.


```yaml
name: Windows.Search.VSS
description: |
  This artifact will find all relevant files in the VSS. Typically
  used to out deduplicated paths for processing by other artifacts.

  NOTE: This used to be more complicated but now delegates to the
  "ntfs_vss" accessor to do all the work.

author: Matt Green - @mgreen27

precondition: SELECT * FROM info() where OS = 'windows'

parameters:
  - name: SearchFilesGlob
    default: C:\Windows\System32\winevt\Logs\Security.evtx
    description: Use a glob to define the files that will be searched.

  - name: VSS_MAX_AGE_DAYS
    type: int
    description: |
      If larger than 0 we restrict VSS age to this many days
      ago. Otherwise we find all VSS.

sources:
  - query: |
      SELECT * FROM glob(globs=SearchFilesGlob, accessor="ntfs_vss")
      ORDER BY OSPath

```
