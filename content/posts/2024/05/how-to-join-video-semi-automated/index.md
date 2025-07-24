+++
title = 'How to join video files in a semi-automated way'
subtitle = 'Simple tech tip'
date = 2024-05-04T08:00:00-07:00
tags = [ "Ffmpeg", "Video Editing", "Tutorial", "Tech Tips"]
categories = ["Tech Related"]
medium = 'https://geraldnguyen.medium.com/how-to-join-video-files-in-a-semi-automated-way-aa22b5f315cc'
+++

TLDR;

{{<dailymotion "x9264lq" >}}

{{< pagebreak >}}


You will need the following:
- ffmpeg: https://ffmpeg.org/
- Windows command prompt

Create a script that use ffmpeg to join video files then save the script and grant it necessary execute permission (on Mac and Linux). Make sure ffmpeg is on executable path, usually the `$PATH` or `%PATH%` variable

On Windows, you can use the below.

```
@echo off
setlocal enabledelayedexpansion

del concat.txt

rem Get the total number of parameters
set "totalParams=0"
for %%# in (%*) do set /a "totalParams += 1"

set /a "to_merge=%totalParams%-1"

echo There are a total of %to_merge% video to merge

rem Loop through all parameters except the last one
set "index=0"
set "outputfile="
for %%f in (%*) do (
  set /a "index+=1"
  if !index! neq %totalParams% (
    set "F=%%f"
    set "F=!F:\=/!"
    set "F=!F:"=!"
    echo file !F!
    echo file !F! >> concat.txt
  ) else ( set "outputfile=%%f" )
)

echo output file %outputfile%

ffmpeg -safe 0 -f concat -i concat.txt -c copy %outputfile%
```

Assume the script is named `concat.cmd`, use the command `concat.cmd <inputfile> <inputfile> <outputfile as the last argument>` to join individual input files to a single output file. For example, to join 2 files `video-1.mp4` and `video-2.mp4` to a single `combine.mp4`, we type the following

```
concat.cmd video-1.mp4 video-2.mp4 combine.mp4
```

The script will join input files in the order you specify and produce a single output file.

Reference:
- https://stackoverflow.com/questions/7333232/how-to-concatenate-two-mp4-files-using-ffmpeg
- https://ffmpeg.org/