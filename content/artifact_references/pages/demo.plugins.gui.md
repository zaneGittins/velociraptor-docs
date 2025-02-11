---
title: Demo.Plugins.GUI
hidden: true
tags: [Client Artifact]
---

A demo plugin showing some GUI features.

This plugin is also used for tests.


```yaml
name: Demo.Plugins.GUI
description: |
  A demo plugin showing some GUI features.

  This plugin is also used for tests.

resources:
  timeout: 20
  ops_per_second: 60
  max_rows: 213
  max_upload_bytes: 545454

parameters:
  - name: ChoiceSelector
    type: choices
    default: First Choice
    choices:
      - First Choice
      - Second Choice
      - Third Choice

  - name: Hashes
    validating_regex: '^\s*([A-F0-9]+\s*)+$'
    description: One or more hashes in hex separated by white space.

  - name: RegularExpression
    type: regex
    default: "."

  - name: MultipleRegularExpression
    type: regex_array
    default: '[".+"]'

  - name: YaraRule
    type: yara

  - name: Flag
    friendly_name: A Flag with a name
    type: bool
    default: True

  - name: Flag2
    type: bool
    default: Y

  - name: Flag3
    type: bool
    default: Y

  - name: OffFlag
    type: bool

  - name: StartDate
    type: timestamp

  - name: StartDate2
    type: timestamp

  - name: StartDate3
    type: timestamp

  - name: CSVData
    type: csv
    default: |
      Column1,Column2
      A,B
      C,D

  - name: CSVData2
    type: csv
    default: |
      Column1,Column2
      A,B
      C,D

  - name: JSONData
    type: json_array
    default: "[]"

  - name: JSONData2
    type: json_array
    default: |
      [{"foo": "bar"}]

  - name: FileUpload1
    type: upload
    description: FileUpload1 can receive a file upload. The upload content will be available in this variable when executing on the client.

  - name: ArtifactSelections
    type: artifactset
    description: A selection of artifact
    artifact_type: CLIENT_EVENT
    default: |
      Artifact
      Windows.Detection.PsexecService
      Windows.Events.ProcessCreation
      Windows.Events.ServiceCreation

column_types:
  - name: Base64Hex
    type: base64hex

sources:
  - query: |
      SELECT base64encode(string="This should popup in a hex editor") AS Base64Hex,
             ChoiceSelector, Flag, Flag2, Flag3,
             OffFlag, StartDate, StartDate2, StartDate3,
             CSVData, CSVData2, JSONData, JSONData2,
             len(list=FileUpload1) AS FileUpload1Length
      FROM scope()

    notebook:
      - type: vql_suggestion
        name: Test Suggestion
        template: |
          /*
          # This is a suggestion notebook cell.

          It should be available from the suggestions list.
          */
          SELECT * FROM info()

      - type: md
        template: |
          # GUI Notebook tests

          The following cells are testing the notebook in the flow. To
          run this test simply collect the `Demo.Plugins.GUI` artifact
          and check the output is correct.

          **Each of the below cells should have a H2 heading**

          ## Check that notebook environment variables are populated
          {{ $x := Query "SELECT * FROM items(\
             item=dict(NotebookId=NotebookId, ClientId=ClientId,\
                       FlowId=FlowId, ArtifactName=ArtifactName))" | Expand }}

          {{ range $x }}
          * {{ Get . "_key" }} - {{ Get . "_value" }}
          {{- end -}}

      - type: md
        template: |
          ## Code syntax highlighting for VQL

          ```vql
          SELECT * FROM info()
          ```

      - type: vql
        template: |
          /*
          ## A VQL cell with a heading.
          */
          LET ColumnTypes = dict(
            Time1="timestamp",
            Time2="timestamp",
            Time3="timestamp",
            Time4="timestamp",
            FlowId="flow",
            ClientId="client",
            Data="hex",
            URL="url",
            SafeURL="safe_url", // Present dialog before click.
            Base64Data="base64hex"
          )

          LET Base64Data = base64encode(string="\x00\x01\x20\x32\x12\x10")
          LET URL = "[Google](https://www.google.com)"

          SELECT 1628609690.1 AS Raw,

                 -- float
                 1628609690.1 AS Time1,

                 -- ms as a string
                 "1628609690100" AS Time2,

                 -- ns
                 1628609690100000 AS Time3,

                 -- Standard string form
                 "2021-08-10T15:34:50Z" AS Time4,

                 FlowId, ClientId, URL, URL AS SafeURL, Base64Data,

                 format(format="%02x", args="Hello") AS Data
          FROM scope()

      - type: Markdown
        template: |
          ## Scatter Chart with a named column

          {{ define "ScatterTest" }}
           SELECT X, Name, Y, Y3
          FROM parse_csv(accessor="data", filename='''
          X,Name,Y,Y3
          1,Bob,2,3
          2,Frank,4,6
          3,Mike,6,8
          4,Sally,3,2
           ''')
          {{ end }}
          {{ Query "ScatterTest" | ScatterChart "name_column" "Name" }}

          ## Stacked Bar Chart (Categories are first column)

          {{ define "Test" }}
          SELECT X, Y, Y3
          FROM parse_csv(accessor="data", filename='''
          X,Y,Y3
          Bob,2,3
          Bill,4,6
          Foo,6,8
          Bar,7,2
          ''')
          {{ end }}
          {{ Query "Test" | BarChart "type" "stacked" }}

          ## Time chart with timestamp in first column

          {{ define "TimeTest" }}
          SELECT Timestamp, Y, Y3
          FROM parse_csv(accessor="data", filename='''
          Timestamp,Y,Y3
          2021-10-09,2,3
          2021-10-10,4,6
          2021-10-11,6,8
          2021-10-12,7,2
          ''')
          {{ end }}
          {{ Query "TimeTest" | TimeChart }}

          ## Line chart

          {{ define "LineTest" }}
          SELECT X, Y, Y3
          FROM parse_csv(accessor="data", filename='''
          X,Y,Y3
          1,2,3
          2,4,6
          3,6,8
          4,7,2
          ''')
          {{ end }}
          {{ Query "LineTest" | LineChart }}

      - type: Markdown
        template: |
          ## A Line Chart

          The following should show a CPU load chart of the last 10 min.

          {{ define "Q" }}
            SELECT _ts, CPUPercent
            FROM monitoring(
                  artifact="Server.Monitor.Health/Prometheus",
                  start_time=now() - 10 * 60)
            LIMIT 100
          {{ end }}

          {{ Query "Q" | TimeChart }}

      - type: vql
        template: |
          /*
          ## Adding timelines

          Add a timeline from this time series data. (This only works
          for root org because it relies on server health events).

          */
          SELECT timestamp(epoch=_ts) AS Timestamp, CPUPercent
          FROM monitoring(
            source="Prometheus",
            artifact="Server.Monitor.Health",
            start_time=now() - 10 * 60)

          LET T1 = SELECT
               timestamp(epoch=_ts) AS Timestamp,
               dict(X=CPUPercent, Y=1) AS Dict
          FROM monitoring(
            source="Prometheus",
            artifact="Server.Monitor.Health",
            start_time=now() - 10 * 60)

          -- Add the time series into the timeline.
          SELECT timeline_add(
              key="Timestamp", name="Time 你好世界 'line' &\" ",
              query=T1, timeline="Test \"Timeline 你好世界\""),
           timeline_add(
              key="Timestamp", name="2",
              query=T1, timeline="Test \"Timeline 你好世界\"")
          FROM scope()

      - type: Markdown
        env:
          - key: Timeline
            value: Test "Timeline 你好世界"
        template: |
          ## This super timeline should have two timelines.

          Add a timeline manually and hit refresh on this cell to
          check it is being updated.

          {{ Scope "Timeline" | Timeline }}

      - type: VQL
        template: |
          /*
          # Test table scrolling.

          Check both expanded and contracted states of the cell
          */
          LET zalgo = "1̴̣̜̗̰͇͖͖̞̮͈͍̂͜.̸̢̧̨͙̻̜̰̼̔̿̓̄̀̅͌̈́͒͗̈́̒̕̚͜͠e̶̙̞̬̹̥͖̤̟͑͒̂̀̔͠x̵̛̱̠̳͍̦̘̤̙͚̙͈̬́̈́͂̎̽̇̀͝ę̵̯̦̫͖͖͍͈̟̠͉̥͒̑̐̏̕̚̕͜͠"
          LET Test = "Hellothereongline" + zalgo

          SELECT Test AS Test1, Test AS Test2, Test AS Test3,
                 Test AS Test4, Test AS Test5,
                 Test AS Test11, Test AS Test21,
                 Test AS Test13, Test AS Test14, Test AS Test15,
                 Test AS Test21, Test AS Test22,
                 Test AS Test23, Test AS Test24, Test AS Test25
          FROM range(start=0, end=100, step=1)

      - type: VQL
        template: |
          /*
          # Column types set in the artifact's `column_types` field

          These apply to notebooks automatically without needing to
          define them again.

          * Hash column should right click to VT
          * upload preview should show the uploaded file.

          */

          LET ColumnTypes = dict(`StartDate`='timestamp',
                                 Hex='hex', Upload='preview_upload')
          LET Hex = "B0 EC 48 5F 18 77"

          SELECT Hex, StartDate, hash(accessor="data", path="Hello") AS Hash,
                 upload(accessor="data", file="Hello world",
                        name="test.txt") AS Upload
          FROM source()

```
