---
title: Windows.Memory.Acquisition
hidden: true
tags: [Client Artifact]
---

Acquires a full memory image. We download winpmem and use it to
acquire a full memory image.

NOTE: This artifact usually transfers a lot of data. You should
increase the default timeout to allow it to complete.


```yaml
name: Windows.Memory.Acquisition
description: |
  Acquires a full memory image. We download winpmem and use it to
  acquire a full memory image.

  NOTE: This artifact usually transfers a lot of data. You should
  increase the default timeout to allow it to complete.

tools:
  - name: WinPmem64
    github_project: Velocidex/WinPmem
    github_asset_regex: winpmem_mini_x64.+exe
    serve_locally: true

precondition: SELECT OS From info() where OS = 'windows' AND Architecture = "amd64"

sources:
  - query: |
      SELECT * FROM foreach(
          row={
            SELECT OSPath, tempfile(extension=".raw", remove_last=TRUE) AS Tempfile
            FROM Artifact.Generic.Utils.FetchBinary(ToolName="WinPmem64")
          },
          query={
            SELECT Stdout, Stderr,
                   if(condition=Complete,
                      then=upload(file=Tempfile, name="PhysicalMemory.raw")) As Upload
            FROM execve(argv=[OSPath, Tempfile], sep="\r\n")
        })

```
