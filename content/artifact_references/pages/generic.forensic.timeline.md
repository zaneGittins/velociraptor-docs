---
title: Generic.Forensic.Timeline
hidden: true
tags: [Client Artifact]
---

This artifact generates a timeline of a file glob in bodyfile
format. We currently do not calculate the md5 because it is quite
expensive.


```yaml
name: Generic.Forensic.Timeline
description: |
  This artifact generates a timeline of a file glob in bodyfile
  format. We currently do not calculate the md5 because it is quite
  expensive.

parameters:
  - name: timelineGlob
    default: C:\Users\**
  - name: timelineAccessor
    default: file

sources:
  # For NTFS accessors we write the MFT id as the inode. On windows
  # the file accessor does not give the inode at all.
  - precondition:
      SELECT OS From info() where OS = 'windows' AND timelineAccessor = 'ntfs'
    query: |
        SELECT 0 AS Md5, OSPath,
               Sys.mft as Inode,
               Mode.String AS Mode, 0 as Uid, 0 as Gid, Size,
               Atime, Mtime, Ctime
        FROM glob(globs=timelineGlob, accessor=timelineAccessor)

  # For linux we can get the Inode from Sys.Ino
  - precondition:
      SELECT * From scope() where timelineAccessor = 'file'
    query: |
        SELECT 0 AS Md5, OSPath,
               Sys.Ino as Inode,
               Mode.String AS Mode, Sys.Uid AS Uid, Sys.Gid AS Gid, Size,
               Atime, Mtime, Ctime
        FROM glob(globs=timelineGlob, accessor=timelineAccessor)

```
