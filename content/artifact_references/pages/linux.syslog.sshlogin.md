---
title: Linux.Syslog.SSHLogin
hidden: true
tags: [Client Artifact]
---

Parses the auth logs to determine all SSH login attempts.


```yaml
name: Linux.Syslog.SSHLogin
description: |
  Parses the auth logs to determine all SSH login attempts.

reference:
  - https://www.elastic.co/blog/grokking-the-linux-authorization-logs

type: CLIENT

parameters:
  - name: syslogAuthLogPath
    default: /var/log/{auth.log,secure}*

  - name: SSHGrok
    description: A Grok expression for parsing SSH auth lines.
    default: >-
      %{SYSLOGTIMESTAMP:Timestamp} (?:%{SYSLOGFACILITY} )?%{SYSLOGHOST:logsource} %{SYSLOGPROG}: %{DATA:event} %{DATA:method} for (invalid user )?%{DATA:user} from %{IPORHOST:ip} port %{NUMBER:port} ssh2(: %{GREEDYDATA:system.auth.ssh.signature})?

sources:
  - query: |
      // Basic syslog parsing via GROK expressions.
      SELECT timestamp(string=Event.Timestamp) AS Time,
               Event.ip AS IP,
               Event.event AS Result,
               Event.method AS Method,
               Event.user AS AttemptedUser,
               OSPath
        FROM foreach(
          row={
              SELECT OSPath FROM glob(globs=syslogAuthLogPath)
          }, query={
              SELECT grok(grok=SSHGrok, data=Line) AS Event, OSPath
              FROM parse_lines(filename=OSPath)
              WHERE Event.program = "sshd"
          })

```
