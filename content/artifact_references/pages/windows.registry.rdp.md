---
title: Windows.Registry.RDP
hidden: true
tags: [Client Artifact]
---

This artifact will collect historical RDP server names and MRU items stored 
in each users NTUser.dat

1. Servers - list of all RDP connections that have ever been established by 
this user.   
 UsernameHint shows the username used to connect to the RDP/RDS host.  
 CertHash variable contains the RDP server SSL certificate thumbprint.

2. MRU 10 - Most recently used RDP connections 

UserRegex and SidRegex can be used to target a specific user.


```yaml
name: Windows.Registry.RDP
author: Matt Green - @mgreen27
description: |
   This artifact will collect historical RDP server names and MRU items stored 
   in each users NTUser.dat
   
   1. Servers - list of all RDP connections that have ever been established by 
   this user.   
    UsernameHint shows the username used to connect to the RDP/RDS host.  
    CertHash variable contains the RDP server SSL certificate thumbprint.

   2. MRU 10 - Most recently used RDP connections 
   
   UserRegex and SidRegex can be used to target a specific user.

type: CLIENT

parameters:
   - name: KeyGlob
     default: Software\Microsoft\Terminal Server Client\{Default,Servers}\**
   - name: UserRegex
     default: .
     description: Regex filter to select a target username
     type: regex
   - name: SidRegex
     default: .
     description: Regex filter to select a target SID
     type: regex
     

precondition: SELECT OS From info() where OS = 'windows'
      
sources:
  - name: Servers
    query: |
      LET servers <= SELECT 
            Mtime as LastWriteTime,
            basename(path=OSPath.Dirname) as Server,
            OSPath.Basename as KeyName,
            Data.value as KeyValue,
            Data.data_len as ValueLength,
            OSPath.Dirname.Path as Key,
            OSPath.DelegatePath as HiveName,
            OSPath,
            Username,
            UUID as SID
        FROM Artifact.Windows.Registry.NTUser(KeyGlob=KeyGlob)
        WHERE NOT Data.type = 'Key'
            AND OSPath =~ '''Terminal Server Client\\\\Servers\\'''

      LET find_value(path, sid, keyname ) = SELECT KeyValue,
            format(format='%x',args=read_file(accessor='data',filename=KeyValue,length=ValueLength)) as CertHash
        FROM servers 
        WHERE KeyName = keyname AND Key=path AND SID=sid
        
      LET results = SELECT 
            Username || dirname(path=HiveName).Basename as Username,  
            SID,
            HiveName,
            Key,  
            LastWriteTime,
            Server
        FROM servers
        WHERE Username =~ UserRegex AND SID =~ SidRegex
        GROUP BY SID,Key,HiveName,LastWriteTime
            
      
      SELECT *
        find_value(path=Key,sid=SID,keyname='UsernameHint')[0].KeyValue as UsernameHint,
        find_value(path=Key,sid=SID,keyname='CertHash')[0].CertHash as CertHash
      FROM results

  - name: Mru
    query: |
      LET mru <= SELECT 
            Mtime as LastWriteTime,
            OSPath.Basename as KeyName,
            Data.value as KeyValue,
            OSPath.Dirname.Path as Key,
            OSPath.DelegatePath as HiveName,
            Username,
            UUID as SID
        FROM Artifact.Windows.Registry.NTUser(KeyGlob=KeyGlob)
        WHERE NOT Data.type = 'Key'
            AND OSPath =~ '''Terminal Server Client\\\\Default\\\\MRU'''
    
      LET find_mru(sid) = SELECT KeyValue FROM mru 
        WHERE SID=sid
        
      LET results = SELECT *,
            Username || dirname(path=HiveName).Basename as Username
        FROM mru 
        WHERE Username =~ UserRegex AND SID =~ SidRegex
        GROUP BY Sid,HiveName
      
      SELECT 
        Username,
        SID,  
        HiveName,
        Key,
        LastWriteTime,
        find_mru(sid=SID).KeyValue as Mru
      FROM results
```
