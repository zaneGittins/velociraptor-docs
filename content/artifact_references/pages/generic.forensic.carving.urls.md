---
title: Generic.Forensic.Carving.URLs
hidden: true
tags: [Client Artifact]
---

Carve URLs from files located in a glob. Note that we do not parse
any files - we simply carve anything that looks like a URL.


```yaml
name: Generic.Forensic.Carving.URLs
description: |
  Carve URLs from files located in a glob. Note that we do not parse
  any files - we simply carve anything that looks like a URL.


parameters:
  - name: UrlGlob
    default: |
      ["C:/Documents and Settings/*/Local Settings/Application Data/Google/Chrome/User Data/**",
       "C:/Users/*/AppData/Local/Google/Chrome/User Data/**",
       "C:/Documents and Settings/*/Local Settings/History/**",
       "C:/Documents and Settings/*/Local Settings/Temporary Internet Files/**",
       "C:/Users/*/AppData/Local/Microsoft/Windows/WebCache/**",
       "C:/Users/*/AppData/Local/Microsoft/Windows/INetCache/**",
       "C:/Users/*/AppData/Local/Microsoft/Windows/INetCookies/**",
       "C:/Users/*/AppData/Roaming/Mozilla/Firefox/Profiles/**",
       "C:/Documents and Settings/*/Application Data/Mozilla/Firefox/Profiles/**"
       ]

sources:
  - query: |
        LET matching = SELECT OSPath FROM glob(
            globs=parse_json_array(data=UrlGlob))

        SELECT OSPath, URL FROM foreach(
          row=matching,
          query={
            SELECT OSPath,
                   URL FROM parse_records_with_regex(file=OSPath,
               regex="(?P<URL>https?:\\/\\/[\\w\\.-]+[\\/\\w \\.-]*)")
          })

```
