---
title: Windows.Events.Mutants
hidden: true
tags: [Client Event Artifact]
---

This artifact detects creation of Mutants and triggers an alert. 


```yaml
name: Windows.Events.Mutants
description: |
  This artifact detects creation of Mutants and triggers an alert. 

author: Jos Clephas - @DfirJos

type: CLIENT_EVENT

precondition:
  SELECT * FROM info() WHERE OS =~ "windows"

parameters:
  - name: processRegex
    description: A regex applied to process names.
    default: .
    type: regex
  - name: Period
    type: int
    default: 120
  - name: MutantNameRegex
    default: EvilMutant
    type: regex
  - name: AlertName
    default: "Suspicious mutex created"
  - name: diff
    default: added
  - name: enrich
    description: Enrich mutex with process information. Closely monitor the performance impact if you enable this.
    type: bool
    default: N

sources:
    - query: |
    
        LET processes = SELECT Pid AS ProcPid, Name AS ProcName, Exe FROM process_tracker_pslist() WHERE ProcName =~ processRegex AND int(int=ProcPid) > 0

        LET query_mutant = SELECT * FROM winobj() WHERE Type = "Mutant" AND Name =~ MutantNameRegex 

        LET query_enriched = SELECT * FROM foreach(
          row=processes,
          query={
            SELECT ProcPid, ProcName, Exe, Type, Name, Handle
            FROM handles(pid=int(int=ProcPid), types="Mutant")
          })
        WHERE Type = "Mutant" AND Name =~ MutantNameRegex
        
        LET query_diff = if(condition=enrich, then=query_enriched, else=query_mutant) 
        
        SELECT *, alert(name=AlertName, Name=Name, Type=Type, Exe=Exe) as AlertSent FROM diff(query=query_diff, period=Period, key="Name") WHERE Diff = diff

```
