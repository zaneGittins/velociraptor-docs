---
title: Windows.Applications.ChocolateyPackages
hidden: true
tags: [Client Artifact]
---

Chocolatey packages installed in a system.

```yaml
name: Windows.Applications.ChocolateyPackages
description: Chocolatey packages installed in a system.
parameters:
  - name: ChocolateyInstall
    default: ""

sources:
  - precondition:
      SELECT OS From info() where OS = 'windows'
    query: |
      LET SearchGlob = if(
             condition=ChocolateyInstall,
             then=ChocolateyInstall,

             -- Otherwise just use the environment.
             else=environ(var='ChocolateyInstall')) + '/lib/*/*.nuspec'

      LET files = SELECT OSPath,
              parse_xml(file=OSPath) AS Metadata
              -- Use the ChocolateyInstall parameter if it is set.

          FROM glob(globs=SearchGlob)

      SELECT * FROM if(
        condition=if(condition=ChocolateyInstall,
                     then=ChocolateyInstall,
                     else=environ(var="ChocolateyInstall")),
        then={
            SELECT OSPath,
                   Metadata.package.metadata.id as Name,
                   Metadata.package.metadata.version as Version,
                   Metadata.package.metadata.summary as Summary,
                   Metadata.package.metadata.authors as Authors,
                   Metadata.package.metadata.licenseUrl as License
            FROM files
        })

```
