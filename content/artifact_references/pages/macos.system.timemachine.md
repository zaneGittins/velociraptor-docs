---
title: MacOS.System.TimeMachine
hidden: true
tags: [Client Artifact]
---

This artifact collects information about MacOS Time Machine backups.


```yaml
name: MacOS.System.TimeMachine
description: |
  This artifact collects information about MacOS Time Machine backups.

type: CLIENT

author: Wes Lambert - @therealwlambert

parameters:
  - name: TimeMachineGlob
    default: /Library/Preferences/com.apple.TimeMachine.plist

sources:
  - query: |
      LET TMPlist = SELECT OSPath FROM glob(globs=TimeMachineGlob)
      LET TMDetails =
            SELECT * FROM foreach(
                row=plist(file=OSPath),
                query={ SELECT
                    plist(file=OSPath).LocalizedDiskImageVolumeName AS VolumeName,
                    plist(file=OSPath).AutoBackup AS AutoBackup,
                    plist(file=OSPath).LastDestinationID AS LastDestination,
                    plist(file=OSPath).HostUUIDs[0] AS HostUUID,
                    plist(file=OSPath).Destinations AS Destinations
                    FROM scope()
                }
            )
      SELECT * FROM foreach(row=TMPlist, query=TMDetails)

```
