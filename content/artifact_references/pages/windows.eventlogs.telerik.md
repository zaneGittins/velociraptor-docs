---
title: Windows.EventLogs.Telerik
hidden: true
tags: [Client Artifact]
---

This Artifact will hunt for evidence of Telerik exploitation in the Application
Event Log.

Telerik is a commonly exploited component of IIS web pages that has been
actively targeted by actors via several CVEs. Several tools and attack
capabilities exist making exploitation of vulnerable services trivial. Due to
the nature of the software and typical deployments the patches may require
manual application.

IocRegex enables searching for regex in the whole EventData field.
Output of this artifact is targeted fields from EventID 1309 to provide
context for the hit.

This Artifact will hunt for evidence of Telerik exploitation in the Application Event Log.


```yaml
name: Windows.EventLogs.Telerik
description: |
  This Artifact will hunt for evidence of Telerik exploitation in the Application
  Event Log.

  Telerik is a commonly exploited component of IIS web pages that has been
  actively targeted by actors via several CVEs. Several tools and attack
  capabilities exist making exploitation of vulnerable services trivial. Due to
  the nature of the software and typical deployments the patches may require
  manual application.

  IocRegex enables searching for regex in the whole EventData field.
  Output of this artifact is targeted fields from EventID 1309 to provide
  context for the hit.

  This Artifact will hunt for evidence of Telerik exploitation in the Application Event Log.

author: Matt Green - @mgreen27

reference:
  - https://www.cyber.gov.au/acsc/view-all-content/advisories/advisory-2020-004-remote-code-execution-vulnerability-being-actively-exploited-vulnerable-versions-telerik-ui-sophisticated-actors
  - https://attack.mitre.org/techniques/T1190/

parameters:
  - name: EvtxGlob
    default: '%SystemRoot%\System32\Winevt\Logs\Application.evtx'
  - name: IocRegex
    description: "IOC Regex"
    default: telerik.*\\?type=rau
    type: regex
  - name: WhitelistRegex
    description: "Regex of string to witelist"
    type: regex
  - name: VSSAnalysisAge
    type: int
    default: 0
    description: |
      If larger than zero we analyze VSS within this many days
      ago. (e.g 7 will analyze all VSS within the last week).  Note
      that when using VSS analysis we have to use the ntfs accessor
      for everything which will be much slower.
  - name: DateAfter
    type: timestamp
    description: "search for events after this date. YYYY-MM-DDTmm:hh:ssZ"
  - name: DateBefore
    type: timestamp
    description: "search for events before this date. YYYY-MM-DDTmm:hh:ssZ"

sources:
  - precondition: SELECT OS From info() where OS = 'windows'

    query: |
      LET VSS_MAX_AGE_DAYS <= VSSAnalysisAge
      LET Accessor = if(condition=VSSAnalysisAge > 0, then="ntfs_vss", else="auto")

      -- firstly set timebounds for performance
      LET DateAfterTime <= if(condition=DateAfter,
        then = DateAfter, else = "1600-01-01" )
      LET DateBeforeTime <= if(condition=DateBefore,
        then = DateBefore, else = "2200-01-01" )

      -- expand provided glob into a list of paths on the file system (fs)
      LET fspaths = SELECT OSPath
        FROM glob(globs=expand(path=EvtxGlob), accessor=Accessor)

      -- function returning IOC hits
      LET evtxsearch(PathList) = SELECT * FROM foreach(
            row=PathList,
            query={
                SELECT
                    timestamp(epoch=int(int=System.TimeCreated.SystemTime)) AS EventTime,
                    System.Computer as Computer,
                    System.Channel as Channel,
                    System.EventID.Value as EventID,
                    System.EventRecordID as EventRecordID,
                    EventData.Data[17] as Exception,
                    EventData.Data[16] as User,
                    EventData.Data[15] as Process,
                    EventData.Data[14] as Pid,
                    EventData.Data[21] as SourceIP,
                    EventData.Data[19] as Uri,
                    EventData.Data[11] as SitePath,
                    OSPath
                FROM parse_evtx(filename=OSPath, accessor=Accessor)
                WHERE EventID = 1309
                    AND format(format='%v',args=EventData.Data) =~ IocRegex
                    AND NOT if(condition=WhitelistRegex,
                        then= format(format='%v',args=EventData.Data) =~ WhitelistRegex,
                        else= FALSE )
                    AND EventTime >= DateAfterTime AND EventTime <= DateBeforeTime
            }
          )

        SELECT * FROM evtxsearch(PathList=fspaths)
        GROUP BY EventRecordID, Channel

```
