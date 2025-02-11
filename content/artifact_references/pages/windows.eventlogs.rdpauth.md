---
title: Windows.EventLogs.RDPAuth
hidden: true
tags: [Client Artifact]
---

This artifact will extract Event Logs related to Remote Desktop sessions,
logon and logoff.

Security channel - EventID in 4624,4634 AND LogonType 3, 7, or 10.
Security channel - EventID in 4778,4625,4779, or 4647.
System channel -  EventID 9009.
Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational - EventID 1149.
Microsoft-Windows-TerminalServices-LocalSessionManager/Operational - EventID 23,22,21,24,25,39, or 40.

Best use of this artifact is to collect RDP and Authentication events around
a timeframe of interest and order by EventTime to scope RDP activity.


```yaml
name: Windows.EventLogs.RDPAuth
author: "Matt Green - @mgreen27"
description: |
    This artifact will extract Event Logs related to Remote Desktop sessions,
    logon and logoff.

    Security channel - EventID in 4624,4634 AND LogonType 3, 7, or 10.
    Security channel - EventID in 4778,4625,4779, or 4647.
    System channel -  EventID 9009.
    Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational - EventID 1149.
    Microsoft-Windows-TerminalServices-LocalSessionManager/Operational - EventID 23,22,21,24,25,39, or 40.

    Best use of this artifact is to collect RDP and Authentication events around
    a timeframe of interest and order by EventTime to scope RDP activity.

reference:
  - https://ponderthebits.com/2018/02/windows-rdp-related-event-logs-identification-tracking-and-investigation/

precondition: SELECT OS From info() where OS = 'windows'

parameters:
  - name: Security
    description: path to Security event log.
    default: '%SystemRoot%\System32\Winevt\Logs\Security.evtx'
  - name: System
    description: path to System event log.
    default: '%SystemRoot%\System32\Winevt\Logs\System.evtx'
  - name: LocalSessionManager
    description: path to TerminalServices-LocalSessionManager operational event log.
    default: '%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-TerminalServices-LocalSessionManager%4Operational.evtx'
  - name: RemoteConnectionManager
    description: path to TerminalServices-RemoteConnectionManager operational event log.
    default: '%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-TerminalServices-RemoteConnectionManager%4Operational.evtx'
  - name: DateAfter
    description: "search for events after this date. YYYY-MM-DDTmm:hh:ss Z"
    type: timestamp
  - name: DateBefore
    description: "search for events before this date. YYYY-MM-DDTmm:hh:ss Z"
    type: timestamp
  - name: SourceIPRegex
    default: ".+"
    type: regex
  - name: UserNameRegex
    default: ".+"
    type: regex
  - name: UserNameWhitelist
    default: '\$$'
    type: regex
  - name: VSSAnalysisAge
    type: int
    default: 0
    description: |
      If larger than zero we analyze VSS within this many days
      ago. (e.g 7 will analyze all VSS within the last week).  Note
      that when using VSS analysis we have to use the ntfs accessor
      for everything which will be much slower.

sources:
  - query: |
      LET VSS_MAX_AGE_DAYS <= VSSAnalysisAge
      LET Accessor = if(condition=VSSAnalysisAge > 0, then="ntfs_vss", else="auto")

      -- firstly set timebounds for performance
      LET DateAfterTime <= if(condition=DateAfter,
        then=DateAfter, else=timestamp(epoch="1600-01-01"))
      LET DateBeforeTime <= if(condition=DateBefore,
        then=DateBefore, else=timestamp(epoch="2200-01-01"))

      -- expand provided glob into a list of paths on the file system (fs)
      LET fspaths <= SELECT OSPath
        FROM glob(globs=[
            expand(path=Security),
            expand(path=System),
            expand(path=LocalSessionManager),
            expand(path=RemoteConnectionManager)], accessor=Accessor)

      -- function returning query hits
      LET evtxsearch(PathList) = SELECT * FROM foreach(
            row=PathList,
            query={
                SELECT
                    timestamp(epoch=int(int=System.TimeCreated.SystemTime)) AS EventTime,
                    System.Computer as Computer,
                    System.Channel as Channel,
                    System.EventID.Value as EventID,
                    if(condition= System.Channel='Security',
                        then= EventData.TargetDomainName,
                        else= if(condition= UserData.EventXML.User,
                            then= split(string=UserData.EventXML.User,sep='\\\\')[0],
                            else= if(condition= UserData.EventXML.Param2,
                                then= UserData.EventXML.Param2,
                                else= 'null' ))) as DomainName,
                    if(condition= System.Channel='Security',
                        then= EventData.TargetUserName,
                        else= if(condition= UserData.EventXML.User,
                            then= split(string=UserData.EventXML.User,sep='\\\\')[1],
                            else= if(condition= UserData.EventXML.Param1,
                                then= UserData.EventXML.Param1,
                                else= 'null' ))) as UserName,
                    if(condition= System.Channel='Security',
                        then= if(condition= EventData.LogonType,
                            then= EventData.LogonType,
                            else= 'null' ),
                        else= 'null' ) as LogonType,
                    if(condition= System.Channel='Security',
                        then= if(condition= EventData.IpAddress,
                            then= EventData.IpAddress,
                            else= 'null' ),
                        else= if(condition= System.Channel=~'TerminalServices',
                            then= if(condition= UserData.EventXML.Address,
                                then= UserData.EventXML.Address,
                                else= if(condition= UserData.EventXML.Param3,
                                    then= UserData.EventXML.Param3,
                                    else= 'null')),
                            else= 'null' )) as SourceIP,
                    if(condition= System.Channel=~'TerminalServices|System',
                        then=
                            get(item=dict(
                                `21`='RDP_LOCAL_CONNECTED',
                                `22`='RDP_REMOTE_CONNECTED',
                                `23`='RDP_SESSION_LOGOFF',
                                `24`='RDP_LOCAL_DISCONNECTED',
                                `25`='RDP_REMOTE_RECONNECTION',
                                `39`='RDP_REMOTE_DISCONNECTED_FORMAL',
                                `40`='RDP_REMOTE_DISCONNECTED_REASON',
                                `1149`='RDP_INITIATION_SUCCESSFUL',
                                `9009`='DESKTOPWINDOWMANAGER_CLOSED'),
                                    member=str(str=System.EventID.Value)),
                        else=if(condition= System.EventID.Value = 4624 AND EventData.LogonType = 10,
                            then='RDP_LOGON_SUCCESSFUL_NEW',
                        else=if(condition= System.EventID.Value = 4624 AND EventData.LogonType = 3,
                            then='LOGON_SUCCESSFUL',
                        else=if(condition= System.EventID.Value = 4624 AND EventData.LogonType = 7,
                            then='LOGON_SUCCESSFUL_OLD',
                        else=if(condition= System.EventID.Value = 4625 AND EventData.LogonType = 3,
                            then='LOGON_FAILED',
                        else=if(condition= System.EventID.Value = 4625 AND EventData.LogonType = 10,
                            then='RDP_LOGON_FAILED',
                        else=
                            get(item=dict(
                                `4778`='LOGON_RECONNECT_EXISTING',
                                `4779`='SESSION_DISCONNECT',
                                `4647`='USER_INITIATED_LOGOFF',
                                `4634`='LOGOFF_DISCONNECT'),
                                    member=str(str=System.EventID.Value)
                        ))))))) as Description,
                    get(field="Message") as Message,
                    System.EventRecordID as EventRecordID,
                    OSPath
                FROM parse_evtx(filename=OSPath, accessor=Accessor)
                WHERE
                    ( Channel = 'Security'
                        AND ( (EventID in (4624,4634) AND LogonType in (3,10,7))
                            OR EventID in (4778,4625,4779,4647)))
                    OR ( Channel = 'System' AND EventID = 9009 )
                    OR ( Channel = 'Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational'
                        AND EventID = 1149 )
                    OR ( Channel = 'Microsoft-Windows-TerminalServices-LocalSessionManager/Operational'
                        AND EventID in (23,22,21,24,25,39,40))
                    AND EventTime < DateBeforeTime
                    AND EventTime > DateAfterTime
                    AND if(condition= UserNameWhitelist,
                        then= NOT UserName =~ UserNameWhitelist,
                        else= True)
                    AND UserName =~ UserNameRegex
                    AND SourceIP =~ SourceIPRegex
            }
          )

      SELECT * FROM if(condition=VSSAnalysisAge > 0,
      then={
        SELECT * FROM evtxsearch(PathList=fspaths)
        GROUP BY EventRecordID, Channel
      }, else={
        SELECT * FROM evtxsearch(PathList=fspaths)
      })

```
