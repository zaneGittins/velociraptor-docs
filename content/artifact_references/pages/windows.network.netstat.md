---
title: Windows.Network.Netstat
hidden: true
tags: [Client Artifact]
---

Show information about open sockets. On windows the time when the
socket was first bound is also shown.


```yaml
name: Windows.Network.Netstat
description: |
  Show information about open sockets. On windows the time when the
  socket was first bound is also shown.

sources:
- precondition: SELECT OS From info() where OS = 'windows'
  query: |
    LET processes <= SELECT Name, Pid AS ProcPid FROM pslist()
    SELECT Pid, {
        SELECT Name from processes
        WHERE Pid = ProcPid
      } AS Name, FamilyString as Family,
      TypeString as Type,
      Status,
      Laddr.IP, Laddr.Port,
      Raddr.IP, Raddr.Port,
      Timestamp
    FROM netstat()

```
