---
title: Server.Alerts.Notification
hidden: true
tags: [Server Event Artifact]
---

This artifact forwards alerts from Server.Internal.Alerts to a Slack/Teams/Discord via a Webhook.


```yaml
name: Server.Alerts.Notification
description: |
   This artifact forwards alerts from Server.Internal.Alerts to a Slack/Teams/Discord via a Webhook.

author: Jos Clephas - @DfirJos

type: SERVER_EVENT

parameters:
  - name: SlackToken
    description: The token URL obtained from Slack/Teams/Discord (or basicly any communication-service that supports webhooks). Leave blank to use server metadata. e.g. https://hooks.slack.com/services/XXXX/YYYY/ZZZZ

sources:
  - query: |
        LET token_url = if(
           condition=SlackToken,
           then=SlackToken,
           else=server_metadata().SlackToken)

        LET hits = SELECT * from watch_monitoring(artifact='Server.Internal.Alerts')

        SELECT * FROM foreach(row=hits,
        query={
           SELECT * FROM http_client(
            data=serialize(item=dict(
                text=format(format="Alert: %v | Details: %v | Artifact: %v | ClientId: %v | Timestamp: %v)",
                            args=[name, event_data, artifact, client_id, timestamp])),
                format="json"),
            headers=dict(`Content-Type`="application/json"),
            method="POST",
            url=token_url)
        })

```
