---
layout: post
title:  "windows file metadata"
date:   2019-01-08 14:53:57 +0800
tag:
  - windows
  - iphone
excerpt_separator: Inconsistent Metadata handling for livephotos
---

The videos and photos take with iPhone live photos on the same date are organized in different timeline stories when import to windows photos.  
The photos seems fine bug the videos are totally mess up.  
It seems these metadata are the causes:  
QuickTime:CreationDate, QuickTime:CreateDate, QuickTime:MediaCreateDate, QuickTime:TrackCreateDate

With exiftool you can modify the metadata of the broken files.  
First, define a tag group in the .ExifTool_config file:

```
%Image::ExifTool::Shortcuts::UserDefined = (
    QtDates => ['QuickTime:CreateDate','QuickTime:MediaCreateDate','QuickTime:TrackCreateDate'],
);
```

Then execute  
exiftool -QtDates<QuickTime:CreationDate your_file_name
