---
layout: post
title:  "file metadata"
date:   2019-01-08 14:53:57 +0800
tag:
  - windows
  - iPhone
excerpt_separator: Inconsistent metadata handling for live photos
---

The videos and photos take with iPhone live photos on the same date are organized in different timeline stories when import to windows photos.  
The photos seem fine but the videos are totally mess up.  
It seems these metadata tags are the causes:  
QuickTime:CreationDate, QuickTime:CreateDate, QuickTime:MediaCreateDate, QuickTime:TrackCreateDate

With exiftool you can modify the metadata of the broken files.  
First, define a tag group in the .ExifTool_config file:

```perl
%Image::ExifTool::Shortcuts::UserDefined = (
    QtDates => ['QuickTime:CreateDate','QuickTime:MediaCreateDate','QuickTime:TrackCreateDate'],
);
```

Then execute  
exiftool -r -ee -overwrite_original "-QtDates<QuickTime:CreationDate" -if "defined $QuickTime:CreationDate && $QuickTime:CreationDate lt $QuickTime:CreateDate" -ext mov -v .

Please leave comments if u have better solutions
