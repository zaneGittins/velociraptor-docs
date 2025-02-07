---
title: Windows.ActiveDirectory.BloodHound
hidden: true
tags: [Client Artifact]
---

This artifact allows deployment of the BloodHound collection tool Sharphound.

BloodHound is a popular Active Directory Assessment tool that uses graph
theory to reveal the hidden and often unintended relationships. It can also be
used to identify and eliminate potentially risky domain configuration.

The Sharphound collection is in json format and upload to the server for
additional processing.

RemovePayload - Due to potential malicious use of this tool I have also
included an option to remove payload after execution.

NOTE: Do not run this artifact as an unrestricted hunt. General recommendation
is to run this artifact on only a handful of machines in a typical domain,
then deduplicate output.


```yaml
name: Windows.ActiveDirectory.BloodHound
description: |
   This artifact allows deployment of the BloodHound collection tool Sharphound.

   BloodHound is a popular Active Directory Assessment tool that uses graph
   theory to reveal the hidden and often unintended relationships. It can also be
   used to identify and eliminate potentially risky domain configuration.

   The Sharphound collection is in json format and upload to the server for
   additional processing.

   RemovePayload - Due to potential malicious use of this tool I have also
   included an option to remove payload after execution.

   NOTE: Do not run this artifact as an unrestricted hunt. General recommendation
   is to run this artifact on only a handful of machines in a typical domain,
   then deduplicate output.


author: Matt Green - @mgreen27

reference:
  - https://github.com/BloodHoundAD/BloodHound
  - https://github.com/chryzsh/awesome-bloodhound

required_permissions:
  - EXECVE

tools:
  - name: SharpHound
    url: https://github.com/BloodHoundAD/BloodHound/raw/master/Collectors/SharpHound.exe

type: CLIENT

parameters:
  - name: RemovePayload
    description: Select to remove payload after execution.
    type: bool

sources:
  - precondition:
      SELECT OS From info() where OS = 'windows'

    query: |
      -- obtain hostname for output prefix
      LET hostname <= SELECT Fqdn FROM info()

      -- get context on target binary
      LET payload <= SELECT * FROM Artifact.Generic.Utils.FetchBinary(
                    ToolName="SharpHound")


      -- build tempfolder for output
      LET tempfolder <= tempdir()


      -- execute payload
      LET deploy = SELECT * FROM execve(argv=[payload.OSPath[0],'--outputdirectory',
                tempfolder,'--nozip','--outputprefix',hostname.Fqdn[0] ])


      -- remove payload if selected
      LET remove <= SELECT * FROM if(condition=RemovePayload,
                then={
                    SELECT * FROM execve(argv=['powershell','Remove-Item',
                                            payload.OSPath[0],'-Force' ])
                })


      -- output rows
      SELECT * FROM if(condition= deploy.ReturnCode[0]= 0,
        then={
            SELECT Name, upload(file=OSPath,name=Name)
            FROM glob(globs="/*.json", root=tempfolder)
        },
        else=deploy)

```
