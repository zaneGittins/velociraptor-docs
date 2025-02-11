---
title: MacOS.System.QuarantineEvents
hidden: true
tags: [Client Artifact]
---


This artifact parses the QuarantineEventsV2 database, which provides
information on when a file was downloaded from the internet.


```yaml
name: MacOS.System.QuarantineEvents
description: |

  This artifact parses the QuarantineEventsV2 database, which provides
  information on when a file was downloaded from the internet.

type: CLIENT

author: Wes Lambert - @therealwlambert

parameters:
- name: QuarantineGlob
  default: /Users/*/Library/Preferences/com.apple.LaunchServices.QuarantineEventsV2

precondition:
      SELECT OS From info() where OS = 'darwin'

sources:
  - query: |
      LET QList = SELECT OSPath
        FROM glob(globs=QuarantineGlob)

      LET QEvents = SELECT *
        FROM sqlite(file=OSPath, query="SELECT * from LSQuarantineEvent")

      // Add delta (978307200 seconds between Cocoa timestamp
      // (2020,1,1) and epoch timestamp (1970,1,1)) to provided Cocoa
      // timestamp

      LET QEventsDetails =
          SELECT * FROM foreach(
              row=QEvents,
              query={ SELECT
                  timestamp(epoch=LSQuarantineTimeStamp + 978307200) AS DownloadTime,
                  LSQuarantineDataURLString AS DownloadURL,
                  LSQuarantineOriginURLString AS Origin,
                  LSQuarantineAgentName AS AgentName,
                  LSQuarantineAgentBundleIdentifier AS AgentBundle,
                  split(string=OSPath, sep='/')[2] AS User,
                  LSQuarantineEventIdentifier AS EventUUID
                 FROM scope()
              }
          )

      SELECT * FROM foreach(row=QList, query=QEventsDetails)

```
