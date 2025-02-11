---
title: Linux.Applications.Docker.Version
hidden: true
tags: [Client Artifact]
---

Get Dockers version by connecting to its socket.

```yaml
name: Linux.Applications.Docker.Version
description: Get Dockers version by connecting to its socket.
parameters:
  - name: dockerSocket
    description: |
      Docker server socket. You will normally need to be root to connect.
    default: /var/run/docker.sock
sources:
  - precondition: |
      SELECT OS From info() where OS = 'linux'
    query: |
        LET data = SELECT parse_json(data=Content) as JSON
        FROM http_client(url=dockerSocket + ":unix/version")

        SELECT JSON.Version as Version,
               JSON.ApiVersion as ApiVersion,
               JSON.MinAPIVersion as MinAPIVersion,
               JSON.GitCommit as GitCommit,
               JSON.GoVersion as GoVersion,
               JSON.Os as Os,
               JSON.Arch as Arch,
               JSON.KernelVersion as KernelVersion,
               JSON.BuildTime as BuildTime
        FROM data

```
