---
title: Windows.Forensics.SolarwindsSunburst
hidden: true
tags: [Client Artifact]
---

"SolarWinds.Orion.Core.BusinessLayer.dll is a SolarWinds digitally-signed component of the Orion software framework that contains a backdoor that communicates via HTTP to third party servers."

We can look for evidence of this dll by first performing a YARA search on the MFT across all drives, then applying an additional FireEye-supplied rule against the file found via MFT.


```yaml
name: Windows.Forensics.SolarwindsSunburst

description: |
    "SolarWinds.Orion.Core.BusinessLayer.dll is a SolarWinds digitally-signed component of the Orion software framework that contains a backdoor that communicates via HTTP to third party servers."

    We can look for evidence of this dll by first performing a YARA search on the MFT across all drives, then applying an additional FireEye-supplied rule against the file found via MFT.

reference:
  - https://www.fireeye.com/blog/threat-research/2020/12/evasive-attacker-leverages-solarwinds-supply-chain-compromises-with-sunburst-backdoor.html

author: Wes Lambert - @therealwlambert

tools:
  - name: SunburstYARARules
    url: https://raw.githubusercontent.com/fireeye/sunburst_countermeasures/main/all-yara.yar

parameters:
    - name: yaraMFT
      type: yara
      description: "The term we will use to search the MFT"
      default: |
        rule Hit {
           strings:
             $a = "SolarWinds.Orion.Core.BusinessLayer.dll" wide nocase
           condition:
             any of them
        }
    - name: SizeMax
      type: int64
      description: "Entries in the MFT under this size in bytes."
      default: 1200000
    - name: SizeMin
      type: int64
      description: "Entries in the MFT over this size in bytes."
      default: 1000000

sources:
  - query: |
      LET yara_rules <= SELECT read_file(filename=OSPath) AS Rule,
           basename(path=OSPath) AS ToolName
        FROM Artifact.Generic.Utils.FetchBinary(
             ToolName="SunburstYARARules", IsExecutable=FALSE)

      LET ntfs_drives = SELECT OSPath + '/$MFT'as Path, OSPath AS Device
          FROM glob(globs="/*", accessor="ntfs")

      LET MFTEntries = SELECT * from foreach(
            row=ntfs_drives,
            query={ SELECT Device, String.Offset AS Offset,
               String.HexData AS HexData,
               Device + "\\" + parse_ntfs(device=Device,
                          mft=String.Offset / 1024).OSPath AS FilePath,
               parse_ntfs(device=Device,
                          mft=String.Offset / 1024) AS MFT
            FROM yara(
             rules=yaraMFT, files=Device + "/$MFT",
             end=10000000000,
             number=1000,
             accessor="ntfs")}) WHERE MFT.Size > SizeMin AND MFT.Size < SizeMax

      LET yarasearch = SELECT Rule, String.Offset AS HitOffset,
             str(str=String.Data) AS HitContext,
             FileName,
             File.Size AS Size,
             File.ModTime AS ModTime
        FROM yara(
            rules=yara_rules[0].Rule, key="A",
            files=FilePath)
        LIMIT 1

      LET yarahits = SELECT * FROM if(condition=yara_rules,
        then={
          SELECT *
          FROM foreach(row=MFTEntries,query=yarasearch)
        })

      SELECT *,
        hash(path=FileName) AS Hash
      FROM yarahits

```
