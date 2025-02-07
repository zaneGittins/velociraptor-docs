---
title: Server.Utils.CreateMSI
hidden: true
tags: [Server Artifact]
---

Build an MSI ready for deployment in the current org.


```yaml
name: Server.Utils.CreateMSI
description: |
  Build an MSI ready for deployment in the current org.

type: SERVER

parameters:
  - name: AlsoBuild_x86
    description: Also build 32 bit MSI for deployment.
    type: bool

sources:
- query: |
    LET Build(Target) = repack(
        upload_name=format(
          format='Org_%v_%v',
          args=[org().name, inventory_get(tool=Target).Definition.filename]),
        target=Target,
        config=serialize(format='yaml', item=org()._client_config))

    SELECT * FROM chain(a={
       SELECT Build(Target="VelociraptorWindowsMSI") FROM scope()
    }, b={
       SELECT Build(Target="VelociraptorWindows_x86MSI") FROM scope()
       WHERE AlsoBuild_x86
    })

```
