---
title: Server.Powershell.EncodedCommand
hidden: true
tags: [Server Event Artifact]
---

It is possible to pass powershell an encoded script. This artifact
decodes the scripts.

NOTE: The client must be running the Windows.Events.ProcessCreation
event artifact to retrieve process execution logs.


```yaml
name: Server.Powershell.EncodedCommand
description: |
  It is possible to pass powershell an encoded script. This artifact
  decodes the scripts.

  NOTE: The client must be running the Windows.Events.ProcessCreation
  event artifact to retrieve process execution logs.

type: SERVER_EVENT

sources:
  - query: |
       SELECT ClientId, ParentInfo, CommandLine, Timestamp, utf16(
          string=base64decode(
             string=parse_string_with_regex(
                string=CommandLine,
                regex='-((?i)(en|enc|encode|encodedCommand)) (?P<Encoded>[^ ]+)'
             ).Encoded)) AS Script
        FROM watch_monitoring(artifact='Windows.Events.ProcessCreation')
        WHERE CommandLine =~ '-(en|enc|encode|encodedCommand)'

reports:
  - type: SERVER_EVENT
    template: |

      Encoded Powershell
      ==================

      {{ .Description }}

      ## Decoded Powershell commands.

      {{ Query "SELECT ClientId, { SELECT os_info.fqdn from clients(client_id=ClientId) } AS FQDN, Script FROM source()" | Table }}

```
