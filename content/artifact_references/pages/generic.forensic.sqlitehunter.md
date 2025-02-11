---
title: Generic.Forensic.SQLiteHunter
hidden: true
tags: [Client Artifact]
---

Hunt for SQLite files.

SQLite has become the de-facto standard for storing application data,
in many types of applications:

- Web Browsers
- Operating Systems
- Various applications, such as iMessage, TCC etc

This artifact can hunt for these artifacts in a mostly automated way.
More info at https://github.com/Velocidex/SQLiteHunter


```yaml

name: Generic.Forensic.SQLiteHunter
description: |
    Hunt for SQLite files.
    
    SQLite has become the de-facto standard for storing application data,
    in many types of applications:
    
    - Web Browsers
    - Operating Systems
    - Various applications, such as iMessage, TCC etc
    
    This artifact can hunt for these artifacts in a mostly automated way.
    More info at https://github.com/Velocidex/SQLiteHunter
    
    

column_types:
- name: Image
  type: preview_upload

export: |
  LET SPEC <= "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"
  LET Specs <= parse_json(data=gunzip(string=base64decode(string=SPEC)))
  LET CheckHeader(OSPath) = read_file(filename=OSPath, length=12) = "SQLite forma"
  LET Bool(Value) = if(condition=Value, then="Yes", else="No")

  -- In fast mode we check the filename, then the header then run the sqlite precondition
  LET matchFilename(SourceName, OSPath) = OSPath =~ get(item=Specs.sources, field=SourceName).filename
    AND CheckHeader(OSPath=OSPath)
    AND Identify(SourceName= SourceName, OSPath= OSPath)
    AND log(message=format(format="%v matched by filename %v",
            args=[OSPath, get(item=Specs.sources, field=SourceName).filename]))

  -- If the user wanted to also upload the file, do so now
  LET MaybeUpload(OSPath) = if(condition=AlsoUpload, then=upload(file=OSPath)) OR TRUE

  LET Identify(SourceName, OSPath) = SELECT if(
    condition=CheckHeader(OSPath=OSPath),
    then={
      SELECT *
      FROM sqlite(file=OSPath, query=get(item=Specs.sources, field=SourceName).id_query)
    }) AS Hits
  FROM scope()
  WHERE if(condition=Hits[0].Check = get(item=Specs.sources, field=SourceName).id_value,
    then= log(message="%v was identified as %v",
            args=[OSPath, get(item=Specs.sources, field=SourceName).Name]),
    else=log(message="%v was not identified as %v (got %v, wanted %v)",
             args=[OSPath, get(item=Specs.sources, field=SourceName).Name, str(str=Hits),
                   get(item=Specs.sources, field=SourceName).id_value]) AND FALSE)

  LET ApplyFile(SourceName) = SELECT * FROM foreach(row={
     SELECT OSPath FROM AllFiles
     WHERE if(condition=MatchFilename,  then=matchFilename(SourceName=SourceName, OSPath=OSPath),
      else=Identify(SourceName= SourceName, OSPath= OSPath))

  }, query={
     SELECT *, OSPath FROM sqlite(
        file=OSPath, query=get(item=Specs.sources, field=SourceName).SQL)
  })

  -- Filter for matching files without sqlite checks.
  LET FilterFile(SourceName) =
     SELECT OSPath FROM AllFiles
     WHERE if(condition=MatchFilename,
              then=OSPath =~ get(item=Specs.sources, field=SourceName).filename)

  -- Build a regex for all enabled categories.
  LET all_categories = SELECT _value FROM foreach(row=["All","MacOS","Chrome","Browser","Firefox","Edge","InternetExplorer","Windows"]) WHERE get(field=_value)
  LET category_regex <= join(sep="|", array=all_categories._value)
  LET AllGlobs <= filter(list=Specs.globs, condition="x=> x.tags =~ category_regex")
  LET _ <= log(message="Globs for category %v is %v", args=[category_regex, CustomGlob || AllGlobs.glob])
  LET AllFiles <= SELECT OSPath FROM glob(globs=CustomGlob || AllGlobs.glob)
    WHERE NOT IsDir AND MaybeUpload(OSPath=OSPath)

parameters:
- name: MatchFilename
  description: |
    If set we use the filename to detect the type of sqlite file.
    When unset we use heristics (slower)
  type: bool
  default: Y

- name: CustomGlob
  description: Specify this glob to select other files


- name: All
  description: Select targets with category All
  type: bool
  default: Y


- name: MacOS
  description: Select targets with category MacOS
  type: bool
  default: N


- name: Chrome
  description: Select targets with category Chrome
  type: bool
  default: N


- name: Browser
  description: Select targets with category Browser
  type: bool
  default: N


- name: Firefox
  description: Select targets with category Firefox
  type: bool
  default: N


- name: Edge
  description: Select targets with category Edge
  type: bool
  default: N


- name: InternetExplorer
  description: Select targets with category InternetExplorer
  type: bool
  default: N


- name: Windows
  description: Select targets with category Windows
  type: bool
  default: N


- name: SQLITE_ALWAYS_MAKE_TEMPFILE
  type: bool
  default: Y

- name: AlsoUpload
  description: If specified we also upload the identified file.
  type: bool

sources:
- name: AllFiles
  query: |
    SELECT * FROM AllFiles


- name: iMessage_Profiles
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="iMessage_Profiles")
    SELECT timestamp(epoch=date / 1000000000 + 978307200) AS Timestamp, *
    FROM Rows
    


- name: Chromium Browser Autofill_Profiles
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser Autofill_Profiles")
    SELECT GUID,
      timestamp(epoch= date_modified) AS DateModified,
      timestamp(epoch= use_date) AS UseDate,
      FirstName, MiddleName, LastName, EmailAddress,
      PhoneNumber, CompanyName, StreetAddress,
      City, State, ZipCode, UseCount, OSPath
    FROM Rows
    


- name: Chromium Browser Autofill_Masked Credit Cards
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser Autofill_Masked Credit Cards")
    SELECT * FROM Rows


- name: Chromium Browser Bookmarks
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="Chromium Browser Bookmarks")
    -- Recursive function to report the details of a folder
    LET ReportFolder(Data, BaseName) = SELECT * FROM chain(a={
      -- First row emit the data about the actual folder
      SELECT BaseName + " | " + Data.name AS Name,
             timestamp(winfiletime=int(int=Data.date_added) * 10) AS DateAdded,
             timestamp(winfiletime=int(int=Data.date_last_used) * 10) AS DateLastUsed,
             Data.type AS Type,
             Data.url || ""  AS URL
      FROM scope()
    },
    b={
       -- If this folder has children recurse into it
       SELECT * FROM foreach(row={
          SELECT _value FROM items(item=Data.children)
       },  query={
          SELECT * FROM ReportFolder(Data=_value, BaseName=BaseName + " | " + Data.name)
       })
    })
    
    LET MatchingFiles = SELECT OSPath, parse_json(data=read_file(filename=OSPath)) AS Data
    FROM Rows
    
    SELECT * FROM foreach(row=MatchingFiles, query={
      SELECT * FROM chain(
      a={
        SELECT OSPath, *, "bookmark_bar" AS Type
        FROM ReportFolder(Data=Data.roots.bookmark_bar, BaseName="")
      },
      b={
        SELECT OSPath, *, "other" AS Type
        FROM ReportFolder(Data=Data.roots.other, BaseName="")
      },
      c={
        SELECT OSPath, *, "synced" AS Type
        FROM ReportFolder(Data=Data.roots.synced, BaseName="")
      })
    })
    


- name: Chromium Browser_Cookies
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser_Cookies")
    SELECT timestamp(winfiletime=(creation_utc * 10) || 0) AS CreationUTC,
           timestamp(winfiletime=(expires_utc * 10) || 0) AS ExpiresUTC,
           timestamp(winfiletime=(last_access_utc * 10) || 0) AS LastAccessUTC,
           HostKey, Name, Path,
           Bool(Value=is_secure) AS IsSecure,
           Bool(Value=is_httponly) AS IsHttpOnly,
           Bool(Value=has_expires) AS HasExpiration,
           Bool(Value=is_persistent) AS IsPersistent,
           Priority, SourcePort, OSPath
    FROM Rows
    


- name: Chromium Browser Extensions
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="Chromium Browser Extensions")
    -- Resolve the message string against the Locale dict
    LET ResolveName(Message, Locale) = get(item=Locale,
          field=lowcase(string=parse_string_with_regex(regex="^__MSG_(.+)__$", string=Message).g1),
          default=Message).message || Message
    
    -- Read the manifest files
    LET ManifestData = SELECT OSPath, parse_json(data=read_file(filename=OSPath)) AS Manifest
    FROM Rows
    
    -- Find the Locale file to help with.
    LET LocaleData = SELECT *, if(condition=Manifest.default_locale, else=dict(),
         then=parse_json(data=read_file(
            filename=OSPath.Dirname + "_locales" + Manifest.default_locale + "messages.json"))) AS Locale
    FROM ManifestData
    
    LET GetIcon(Manifest) = Manifest.icons.`128` || Manifest.icons.`64` || Manifest.icons.`32` || Manifest.icons.`16`
    
    SELECT OSPath, Manifest.author.email AS Email,
      ResolveName(Message = Manifest.name, Locale=Locale) AS name,
      ResolveName(Message = Manifest.description, Locale=Locale) AS description,
      Manifest.oauth2.scopes as Scopes,
      Manifest.permissions as Permissions,
      Manifest.key as Key, if(condition=GetIcon(Manifest=Manifest),
                then=upload(file=OSPath.Dirname + GetIcon(Manifest=Manifest))) AS Image,
      Manifest AS _Manifest
    FROM LocaleData
    


- name: Chromium Browser Favicons
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser Favicons")
    SELECT ID, IconID,
      timestamp(winfiletime= (LastUpdated * 10) || 0) AS LastUpdated,
      PageURL, FaviconURL,
      upload(accessor="data",
         file=_image,
         name=format(format="Image%v.png", args=ID)) AS Image,
      OSPath as _OSPath
    FROM Rows
    


- name: Chromium Browser History_Visits
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser History_Visits")
    SELECT ID,
       timestamp(winfiletime=(visit_time * 10) || 0) AS VisitTime,
       timestamp(winfiletime=(last_visit_time * 10) || 0) AS LastVisitedTime,
       URLTitle, URL, VisitCount, TypedCount,
       if(condition=hidden =~ '1', then="Yes", else="No") AS Hidden,
       VisitID, FromVisitID,
       visit_duration / 1000000 AS VisitDurationInSeconds,
       OSPath
    FROM Rows
    


- name: Chromium Browser History_Downloads
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser History_Downloads")
    LET StateLookup <= dict(`0`='In Progress', `1`='Complete', `2`="Cancelled", `3`="Interrupted", `4`="Interrupted")
    LET DangerType <= dict(`0`='Not Dangerous', `1`="Dangerous", `2`='Dangerous URL', `3`='Dangerous Content',
        `4`='Content May Be Malicious', `5`='Uncommon Content', `6`='Dangerous But User Validated',
        `7`='Dangerous Host', `8`='Potentially Unwanted', `9`='Whitelisted by Policy')
    LET InterruptReason <= dict(`0`= 'No Interrupt', `1`= 'File Error', `2`='Access Denied', `3`='Disk Full',
      `5`='Path Too Long',`6`='File Too Large', `7`='Virus', `10`='Temporary Problem', `11`='Blocked',
      `12`='Security Check Failed', `13`='Resume Error', `20`='Network Error', `21`='Operation Timed Out',
      `22`='Connection Lost', `23`='Server Down', `30`='Server Error', `31`='Range Request Error',
      `32`='Server Precondition Error', `33`='Unable to get file', `34`='Server Unauthorized',
      `35`='Server Certificate Problem', `36`='Server Access Forbidden', `37`='Server Unreachable',
      `38`='Content Length Mismatch', `39`='Cross Origin Redirect', `40`='Cancelled', `41`='Browser Shutdown',
      `50`='Browser Crashed')
    
    SELECT ID, GUID, CurrentPath, TargetPath, OriginalMIMEType, ReceivedBytes, TotalBytes,
      timestamp(winfiletime=(start_time * 10) || 0) AS StartTime,
      timestamp(winfiletime=(end_time * 10) || 0) AS EndTime,
      timestamp(winfiletime=(opened * 10) || 0) AS Opened,
      timestamp(winfiletime=(last_access_time * 10) || 0) AS LastAccessTime,
      timestamp(epoch=last_modified) AS LastModified,
      get(item=StateLookup, field=str(str=state), default="Unknown") AS State,
      get(item=DangerType, field=str(str=danger_type), default="Unknown") AS DangerType,
      get(item=InterruptReason, field=str(str=interrupt_reason), default="Unknown") AS InterruptReason,
      ReferrerURL, SiteURL, TabURL, TabReferrerURL, DownloadURL, OSPath
    FROM Rows
    


- name: Chromium Browser History_Keywords
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser History_Keywords")
    SELECT KeywordID, URLID,
       timestamp(winfiletime=(last_visit_time * 10) || 0) AS LastVisitedTime,
       KeywordSearchTerm, Title, URL, OSPath
    FROM Rows
    


- name: Chromium Browser Media_History
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser Media_History")
    SELECT ID, URL, WatchTimeSeconds,
       Bool(Value=has_video) AS HasVideo,
       Bool(Value=has_audio) AS HasAudio,
       timestamp(winfiletime=last_updated_time_s || 0) AS LastUpdated,
       OriginID, OSPath
    FROM Rows
    


- name: Chromium Browser Media_Playback Session
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser Media_Playback Session")
    SELECT ID,
      timestamp(winfiletime=last_updated_time_s || 0) AS LastUpdated, URL,
      duration_ms / 1000 AS DurationInSeconds,
      position_ms / 1000 AS PositionInSeconds,
      Title, Artist, Album, SourceTitle, OriginID, OSPath
    FROM Rows
    


- name: Chromium Browser Network_Predictor
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser Network_Predictor")
    SELECT * FROM Rows
    


- name: Chromium Browser Shortcuts
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser Shortcuts")
    SELECT ID,
      timestamp(winfiletime= (last_access_time * 10) || 0) AS LastAccessTime,
      TextTyped, FillIntoEdit, URL, Contents,
      Description, Type, Keyword, TimesSelectedByUser, OSPath
    FROM Rows
    


- name: Chromium Sessions_Sessions
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Sessions_Sessions")
    SELECT * FROM info()
    


- name: Chromium Browser Top Sites
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Chromium Browser Top Sites")
    SELECT * FROM Rows
    


- name: Firefox Places
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Firefox Places")
    LET BookmarkTypes <= dict(`1`="URL", `2`="Folder", `3`="Separator")
    SELECT ID, ParentID,
       get(item= BookmarkTypes, field=str(str=type), default="Unknown") AS Type,
       timestamp(epoch=dateAdded) AS DateAdded,
       timestamp(epoch=lastModified) AS LastModified,
       Position, Title, URL, ForeignKey, OSPath
    FROM Rows
    


- name: Firefox Places_Downloads
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Firefox Places_Downloads")
    SELECT PlaceID, Content,
       timestamp(epoch=dateAdded) AS DateAdded,
       timestamp(epoch=lastModified) AS LastModified,
       OSPath
    FROM Rows
    


- name: Firefox Places_History
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Firefox Places_History")
    LET VisitType <= dict(`1`='TRANSITION_LINK', `2`='TRANSITION_TYPED', `3`='TRANSITION_BOOKMARK',
      `4`='TRANSITION_EMBED', `5`= 'TRANSITION_REDIRECT_PERMANENT', `6`='TRANSITION_REDIRECT_TEMPORARY',
      `7`='TRANSITION_DOWNLOAD', `8`='TRANSITION_FRAMED_LINK', `9`='TRANSITION_RELOAD')
    
    SELECT VisitID, FromVisitID,
       timestamp(epoch= last_visit_date) AS LastVisitDate,
       VisitCount, URL, Title, Description,
       get(item= VisitType, field=str(str=visit_type), default="Unknown") AS VisitType,
       Bool(Value=hidden) AS Hidden,
       Bool(Value=types) AS Typed,
       Frecency, PreviewImageURL, OSPath
    FROM Rows
    


- name: Firefox Cookies
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Firefox Cookies")
    SELECT ID, Host, Name, Value,
       timestamp(epoch= creationTime) AS CreationTime,
       timestamp(epoch= lastAccessed) AS LastAccessedTime,
       timestamp(epoch= expiry) AS Expiration,
       Bool(Value= isSecure) AS IsSecure,
       Bool(Value= isHttpOnly) AS IsHTTPOnly, OSPath
    FROM Rows
    


- name: Firefox Downloads
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Firefox Downloads")
    SELECT ID, Name, MIMEType, Source, Target,
       timestamp(epoch= startTime) AS StartTime,
       timestamp(epoch= endTime) AS EndTime,
       timestamp(epoch= expiry) AS Expiration,
       CurrentBytes, MaxBytes, OSPath
    FROM Rows
    


- name: Firefox Favicons
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Firefox Favicons")
    SELECT ID, PageURL, FaviconURL,
       timestamp(epoch= expire_ms) AS Expiration,
       OSPath
    FROM Rows
    


- name: Firefox Form History
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Firefox Form History")
    SELECT ID, FieldName, Value, TimesUsed,
       timestamp(epoch= firstUsed) AS FirstUsed,
       timestamp(epoch= lastUsed) AS LastUsed,
       GUID, OSPath
    FROM Rows
    


- name: IE or Edge WebCacheV01_All Data
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="IE or Edge WebCacheV01_All Data")
    LET MatchingFiles = SELECT OSPath FROM Rows
    
    LET Containers(OSPath) = SELECT Table
    FROM parse_ese_catalog(file=OSPath)
    WHERE Table =~ "Container_"
    GROUP BY Table
    
    LET AllHits(OSPath) = SELECT * FROM foreach(row={
        SELECT * FROM Containers(OSPath=OSPath)
    }, query={
       SELECT timestamp(winfiletime=ExpiryTime) AS ExpiryTime,
          timestamp(winfiletime=ModifiedTime) AS ModifiedTime,
          timestamp(winfiletime=AccessedTime) AS AccessedTime, Url, *
       FROM parse_ese(file=OSPath, table=Table)
    })
    
    SELECT * FROM foreach(row=MatchingFiles, query={
      SELECT * FROM AllHits(OSPath=OSPath)
    })
    


- name: IE or Edge WebCacheV01_Highlights
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="IE or Edge WebCacheV01_Highlights")
    SELECT * FROM foreach(row=MatchingFiles, query={
      SELECT AccessedTime, ModifiedTime, ExpiryTime, Url
      FROM AllHits(OSPath=OSPath)
    })
    


- name: MacOS Applications Cache
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="MacOS Applications Cache")
    SELECT
       time_stamp AS Timestamp,
       OSPath.Base AS Application,
       entry_ID AS EntryID,
       version AS Version,
       hash_value AS Hash,
       storage_policy AS StoragePolicy,
       request_key AS URL,
       plist(file=request_object, accessor="data") AS Request,
       plist(file=response_object, accessor="data") AS Response,
       partition AS Partition,
       OSPath
    FROM Rows
    


- name: MacOS NetworkUsage
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="MacOS NetworkUsage")
    SELECT timestamp(epoch= ZTIMESTAMP + 978307200) AS Timestamp,
      timestamp(epoch= ZFIRSTTIMESTAMP + 978307200) AS FirstTimestamp,
      timestamp(epoch= LIVE_USAGE_TIMESTAMP + 978307200) AS LiveUsageTimestamp,
      ZBUNDLENAME AS BundleID,
      ZPROCNAME AS ProcessName,
      ZWIFIIN AS WifiIn,
      ZWIFIOUT AS WifiOut,
      ZWWANIN AS WanIn,
      ZWWANOUT AS WandOut,
      ZWIREDIN AS WiredIn,
      ZWIREDOUT AS WiredOut,
      ZXIN AS _XIn,
      ZXOUT AS _XOut,
      Z_PK AS LiveUsageTableID
    FROM Rows
    


- name: MacOS Notes
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="MacOS Notes")
    SELECT Key AS _Key,
     OSPath[1] AS User,
     Note,
     Title,
     Snippet,
     NoteID AS _NoteID,
     timestamp(cocoatime=CreatedTS) AS CreatedTime,
     timestamp(cocoatime=LastOpenedDate) AS LastOpenedTime,
     timestamp(cocoatime=DirModificationDate) AS LastDirModifcation,
     Account AS _Account,
     Directory,
     DirectoryID,
     AttachmentName,
     AttachmentSize,
     AttachmentUUID,
     if(condition=AttachmentUUID,
        then=OSPath[:2] + '/Library/Group Containers/group.com.apple.notes/Accounts/LocalAccount/Media/' + AttachmentUUID + '/' + AttachmentName) AS AttachmentLocation,
     AccountName AS _AccountName,
     AccountID AS _AccountID,
     AccountType AS _AccountType,
     gunzip(string=Data) AS Data,
     OSPath
    FROM Rows
    


- name: Windows Activities Cache_ActivityPackageId
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Windows Activities Cache_ActivityPackageId")
    SELECT format(format="%0X-%0X-%0X-%0X-%0X", args=[
      ActivityId[0:4], ActivityId[4:6], ActivityId[6:8],
      ActivityId[8:10], ActivityId[10:] ]) AS ActivityId,
      Platform, PackageName, ExpirationTime, OSPath
    FROM Rows
    


- name: Windows Activities Cache_Clipboard
  query: |
    LET Rows = SELECT * FROM ApplyFile(SourceName="Windows Activities Cache_Clipboard")
    SELECT
      CreatedTime,
      LastModifiedTime,
      LastModifiedOnClient,
      StartTime,
      EndTime,
      Payload,
      OSPath[1] AS User,
      base64decode(string=parse_json_array(data=ClipboardPayload)[0].content) AS ClipboardPayload,
      OSPath AS Path,
      Mtime
    FROM Rows
    


- name: Windows Search Service_SystemIndex_Gthr
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="Windows Search Service_SystemIndex_Gthr")
    LET MatchingFiles = SELECT OSPath FROM Rows
    
    LET FormatTimeB(T) = timestamp(winfiletime=parse_binary(
       filename=T, accessor="data", struct="uint64b"))
    
    LET FormatTime(T) = timestamp(winfiletime=parse_binary(
       filename=T, accessor="data", struct="uint64"))
    
    LET FormatSize(T) = parse_binary(
       filename=T, accessor="data", struct="uint64")
    
    SELECT * FROM foreach(row=MatchingFiles, query={
       SELECT ScopeID, DocumentID, SDID,
          FormatTimeB(T=LastModified) AS LastModified,
          FileName
       FROM parse_ese(file=OSPath, table= "SystemIndex_Gthr")
    })
    


- name: Windows Search Service_SystemIndex_GthrPth
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="Windows Search Service_SystemIndex_GthrPth")
    SELECT * FROM foreach(row=MatchingFiles, query={
       SELECT Scope, Parent, Name
       FROM parse_ese(file=OSPath, table= "SystemIndex_GthrPth")
    })
    


- name: Windows Search Service_SystemIndex_PropertyStore
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="Windows Search Service_SystemIndex_PropertyStore")
    LET X = scope()
    
    -- The PropertyStore columns look like
    -- <random>-ProperName so we strip the
    -- random part off to display it properly.
    LET FilterDict(Dict) = to_dict(item={
      SELECT split(sep_string="-", string=_key)[1] || _key AS _key, _value
      FROM items(item=Dict)
    })
    
    LET PropStore(OSPath) = SELECT *,
       FormatTime(T=X.System_Search_GatherTime) AS System_Search_GatherTime,
       FormatSize(T=X.System_Size) AS System_Size,
       FormatTime(T=X.System_DateModified) AS System_DateModified,
       FormatTime(T=X.System_DateAccessed) AS System_DateAccessed,
       FormatTime(T=X.System_DateCreated) AS System_DateCreated
    FROM foreach(row={
       SELECT *, FilterDict(Dict=_value) AS _value
       FROM items(item={
         SELECT * FROM parse_ese(file=OSPath, table="SystemIndex_PropertyStore")
      })
    }, column="_value")
    
    SELECT * FROM foreach(row=MatchingFiles, query={
       SELECT *
       FROM PropStore(OSPath=OSPath)
    })
    


- name: Windows Search Service_SystemIndex_PropertyStore_Highlights
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="Windows Search Service_SystemIndex_PropertyStore_Highlights")
    SELECT * FROM foreach(row=MatchingFiles, query={
       SELECT WorkID,
          System_Search_GatherTime,
          System_Size,
          System_DateModified,
          System_DateCreated,
          X.System_FileOwner AS System_FileOwner,
          X.System_ItemPathDisplay AS System_ItemPathDisplay,
          X.System_ItemType AS System_ItemType,
          X.System_FileAttributes AS System_FileAttributes,
          X.System_Search_AutoSummary AS System_Search_AutoSummary
       FROM PropStore(OSPath=OSPath)
    })
    


- name: Windows Search Service_BrowsingActivity
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="Windows Search Service_BrowsingActivity")
    SELECT * FROM foreach(row=MatchingFiles, query={
       SELECT X.ItemPathDisplay AS ItemPathDisplay,
          X.Activity_ContentUri AS Activity_ContentUri,
          X.Activity_Description AS Activity_Description
       FROM PropStore(OSPath=OSPath)
       WHERE Activity_ContentUri
    })
    


- name: Windows Search Service_UserActivityLogging
  query: |
    LET Rows = SELECT * FROM FilterFile(SourceName="Windows Search Service_UserActivityLogging")
    SELECT * FROM foreach(row=MatchingFiles, query={
       SELECT X.System_ItemPathDisplay AS System_ItemPathDisplay,
           FormatTime(T=X.ActivityHistory_StartTime) AS ActivityHistory_StartTime,
           FormatTime(T=X.ActivityHistory_EndTime) AS ActivityHistory_EndTime,
           X.ActivityHistory_AppId AS ActivityHistory_AppId
       FROM PropStore(OSPath=OSPath)
       WHERE ActivityHistory_AppId
    })
    




```
