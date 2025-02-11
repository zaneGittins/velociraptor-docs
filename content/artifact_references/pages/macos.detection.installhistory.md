---
title: MacOS.Detection.InstallHistory
hidden: true
tags: [Client Artifact]
---

This artifact collects entries from the InstallHistory .plist file


```yaml
name: MacOS.Detection.InstallHistory
description: |
  This artifact collects entries from the InstallHistory .plist file

type: CLIENT

author: Wes Lambert - @therealwlambert

precondition: SELECT OS FROM info() WHERE OS =~ 'darwin'

parameters:
- name: InstallHistoryGlob
  default: /Library/Receipts/InstallHistory.plist

sources:
- name: Install History
  query: |
    LET SWplist = SELECT OSPath FROM glob(globs=InstallHistoryGlob)

    LET SoftwareDetails =
            SELECT * FROM foreach(
                row=plist(file=OSPath),
                query={
                    SELECT
                        get(member="displayName", default="") AS DisplayName,
                        get(member="displayVersion", default="") AS DisplayVersion,
                        get(member="processName", default="") AS ProcessName,
                        get(member="date", default="") AS InstallDate,
                        get(member="contentType", default="") AS ContentType,
                        get(member="packageIdentifiers", default="") AS PackageIdentifiers
                    FROM scope()
            })
    SELECT * FROM foreach(row=SWplist, query=SoftwareDetails)

```
