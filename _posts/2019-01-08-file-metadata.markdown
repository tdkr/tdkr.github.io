---
layout:  post
title:  "Live Photo Metadata"
date:  2019-01-08 14:53:57 +0800
category: System
tags:
  - windows
  - iPhone
  - exiftool
  - livephoto
excerpt_separator: Inconsistent metadata handling for live photos
---

* content
{:toc}

The videos and photos taken with iPhone live photos on the same date are organized in different timeline stories when imported to windows photos.  
The photo seems fine but the videos are totally mess up.  
It seems these metadata tags are the causes:  
QuickTime:CreationDate, QuickTime:CreateDate, QuickTime:MediaCreateDate, QuickTime:TrackCreateDate

With exiftool you can modify the metadata of the broken files batchly.  
First, define a tag group in the .ExifTool_config file:

```perl
%Image::ExifTool::Shortcuts::UserDefined = (
    QtDates => ['QuickTime:CreateDate','QuickTime:MediaCreateDate','QuickTime:TrackCreateDate'],
);
```

Then execute
```bash
exiftool -r -ee -overwrite_original "-QtDates<QuickTime:CreationDate" -if "defined $QuickTime:CreationDate && $QuickTime:CreationDate lt $QuickTime:CreateDate" -ext mov -v .
```

Please leave comments if u have better solutions
