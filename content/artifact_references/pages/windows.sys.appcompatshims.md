---
title: Windows.Sys.AppcompatShims
hidden: true
tags: [Client Artifact]
---

Application Compatibility shims are a way to persist malware. This
table presents the AppCompat Shim information from the registry in a
nice format.


```yaml
name: Windows.Sys.AppcompatShims
description: |
  Application Compatibility shims are a way to persist malware. This
  table presents the AppCompat Shim information from the registry in a
  nice format.

reference:
  - http://files.brucon.org/2015/Tomczak_and_Ballenthin_Shims_for_the_Win.pdf

parameters:
  - name: shimKeys
    default: >-
      HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\InstalledSDB\*
  - name: customKeys
    default: >-
      HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Custom\*\*

sources:
  - precondition:
      SELECT OS From info() where OS = 'windows'
    query: |
        LET installed_sdb <=
           SELECT Key, Key.Name as SdbGUID, DatabasePath,
                  DatabaseType, DatabaseDescription,
                  -- Convert windows file time to unix epoch.
                  (DatabaseInstallTimeStamp / 10000000) - 11644473600 AS DatabaseInstallTimeStamp
           FROM read_reg_key(
             globs=split(string=shimKeys, sep=",[\\s]*"),
             accessor="registry")

        LET result = SELECT * from foreach(
          row={
            SELECT regex_replace(
               source=OSPath,
               replace="$1",
               re="^.+\\\\([^\\\\]+)\\\\[^\\\\]+$") as Executable,
              regex_replace(
               source=Name,
               replace="$1",
               re="(\\{[^}]+\\}).*$") as SdbGUIDRef,
               Name as ExeName
            FROM glob(
              globs=split(string=customKeys, sep=",[\\s]*"),
              accessor="registry")
          },
          query={
            SELECT Executable, DatabasePath, DatabaseType,
                   DatabaseDescription, DatabaseInstallTimeStamp, SdbGUID
            FROM installed_sdb
            WHERE SdbGUID = SdbGUIDRef
          })

        SELECT * from result

```
