---
title: Server.Information.Users
hidden: true
tags: [Server Artifact]
---

List the user names and SIDs on each machine. We get this
information from the last time we collected Windows.Sys.Users. If we
never collected it for this machine, there will be no results.


```yaml
name: Server.Information.Users
description: |
  List the user names and SIDs on each machine. We get this
  information from the last time we collected Windows.Sys.Users. If we
  never collected it for this machine, there will be no results.

type: SERVER

parameters:
  - name: StandardUserAccounts
    description: Well known SIDs to hide from the output.
    default: "(-5..$|S-1-5-18|S-1-5-19|S-1-5-20)"
    type: regex

sources:
  - query: |
        LET clients = SELECT client_id, os_info.fqdn AS Fqdn FROM clients()

        // Get the most recent collection of our user listing.
        LET last_user_listing = SELECT
               session_id AS flow_id,
               active_time, client_id, Fqdn
           FROM flows(client_id=client_id)
           WHERE artifacts_with_results =~'Windows.Sys.Users'
           ORDER BY active_time
           DESC LIMIT 1

        /* For each Windows.Sys.Users collection, extract the user
           names, but hide standard SIDs.
        */
        LET users = SELECT * FROM foreach(
            row=last_user_listing,
            query={
              SELECT Name, UUID, client_id, Fqdn from source(
                 flow_id=flow_id,
                 artifact='Windows.Sys.Users',
                 client_id=client_id)
              WHERE NOT UUID =~ StandardUserAccounts
            })

        SELECT * FROM foreach(row=clients, query=users)

```
