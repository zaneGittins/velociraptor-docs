---
title: Server.Utils.ListUsers
hidden: true
tags: [Server Artifact]
---

This server artifact is used to list all current users and their
permissions and org access.

NOTE: When collected in an org context only users belonging to the
current org are visible. When collected in the context of the root
org, all users in all orgs are visible.


```yaml
name: Server.Utils.ListUsers
description: |
  This server artifact is used to list all current users and their
  permissions and org access.

  NOTE: When collected in an org context only users belonging to the
  current org are visible. When collected in the context of the root
  org, all users in all orgs are visible.

type: SERVER

sources:
  - query: |
      SELECT * FROM gui_users(all_orgs=TRUE)

```
