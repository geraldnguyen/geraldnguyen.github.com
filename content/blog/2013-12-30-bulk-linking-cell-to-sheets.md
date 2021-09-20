---
title: "[Excel] Bulk Linking Cells to Sheets Whose Name Equates Cell's value"
date: 2013-09-10T09:19:42+01:00
lead: "If you have an excel with an index page and many detailed sheets, you may find the below macro useful. The only requirement is to name your sheet with value of your cell/link in the index page"
draft: false
weight: 50
tags: VBA, old blog
---


```
Sub CreateLinkToSheet()
    Dim c As Range
    For Each c In Selection
        ' MsgBox c.Value
        ActiveSheet.Hyperlinks.Add Anchor:=c, Address:="", _
        SubAddress:=c.Value & "!A1", TextToDisplay:=c.Value
    Next c
End Sub
```

**How to run**: Select the cells to create link and run the macro

