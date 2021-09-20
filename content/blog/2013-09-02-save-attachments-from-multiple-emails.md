---
title: "[VBA] Save Attachements from Multiple Emails"
date: 2013-09-02T09:19:42+01:00
draft: false
weight: 50
tags: VBA, old blog
---

**Prerequisite**: Enable Developer tab, macro security and create “C:\mailattachment\” folder if not exists

**Instruction**: Select emails which have attachment to save (e.g. email containing daily report), select Developer, Macro, SaveAttachment

```
Sub SaveAttachment()
    Dim myOlapp As Outlook.Application
    Dim myNameSpace As Outlook.NameSpace
    Dim myFolder As Outlook.MAPIFolder
    Dim myItem As Outlook.MailItem
    Dim myAttachment As Outlook.Attachment
    Dim I As Long
      
    Set myOlapp = CreateObject("Outlook.Application")
    'Set myNameSpace = myOlapp.GetNamespace("MAPI")
    'Set myFolder = myNameSpace.GetDefaultFolder(olFolderInbox)
    'Set myFolder = myFolder.Folders("Ethsys")
    'Set myFolder = myFolder.Folders("Ethsys Operation Alert")
      
    'For Each myItem In myFolder.Items
        'If myItem.Attachments.Count <> 0 Then
            'For Each myAttachment In myItem.Attachments
                'MsgBox myAttachment.FileName
            'Next
        'End If
    'Next
     
     
    For Each myItem In Application.ActiveExplorer.Selection
        If myItem.Attachments.Count <> 0 Then
            For Each myAttachment In myItem.Attachments
                'MsgBox myAttachment.FileName
                myAttachment.SaveAsFile ("C:\mailattachment\" & myAttachment.DisplayName)
            Next
        End If
    Next
     
End Sub
```