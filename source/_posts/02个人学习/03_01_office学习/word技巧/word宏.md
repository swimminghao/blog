---
title: word宏
tags:
  - word
categories: 技巧
abbrlink: 98fd4232
date: 2023-10-07 18:06:00
---

# word宏

```aidl
Sub 首行缩进()
'
' 首行缩进 宏
'
'
    With Selection.ParagraphFormat
        .LeftIndent = CentimetersToPoints(0)
        .RightIndent = CentimetersToPoints(0)
        .SpaceBefore = 0
        .SpaceBeforeAuto = False
        .SpaceAfter = 0
        .SpaceAfterAuto = False
        .LineSpacingRule = wdLineSpace1pt5
        .Alignment = wdAlignParagraphLeft
        .WidowControl = False
        .KeepWithNext = False
        .KeepTogether = False
        .PageBreakBefore = False
        .NoLineNumber = False
        .Hyphenation = 
        .FirstLineIndent = CentimetersToPoints(0.35)
        .OutlineLevel = wdOutlineLevelBodyText
        .CharacterUnitLeftIndent = 0
        .CharacterUnitRightIndent = 0
        .CharacterUnitFirstLineIndent = 2
        .LineUnitBefore = 0
        .LineUnitAfter = 0
        .MirrorIndents = False
        .TextboxTightWrap = wdTightNone
        .CollapsedByDefault = False
        .AutoAdjustRightIndent = True
        .DisableLineHeightGrid = False
        .FarEastLineBreakControl = True
        .WordWrap = True
        .HangingPunctuation = True
        .HalfWidthPunctuationOnTopOfLine = False
        .AddSpaceBetweenFarEastAndAlpha = True
        .AddSpaceBetweenFarEastAndDigit = True
        .BaseLineAlignment = wdBaselineAlignAuto
    End With
End Sub
Sub InsertCaption()  '修改系统插入“题注”命令

   '功能：自动删除标签与编号间的空格（英文除外），并在题注数字后添加一个空格；适用于：Word 2003 - 2013，不兼容WPS文字！
  '真正从原理上协同系统插入题注，无任何前提条件；用户照常插入题注即可，甚至感觉不到程序的存在！
   'Endlesswx于2015年8月4日

  '另,如果插入的始终未域代码而不是数字，非程序问题，Alt+F9一次即可

   Dim Lab As String, startPt As Long, endPt As Long, myrang As Range
   'On Error Resume Next  '发生错误时让程序继续执行下一句代码
'    Application.ScreenUpdating = False     '关闭屏幕更新，2013在此处关闭更新会导致输入框灰色不可选，故修正在调出对话框之后

   startPt = Selection.Start  'startPt标注起始点

   '将if条件隐藏隐藏即可实现----手动替换题注空格
   If Application.Dialogs(357).Show = -1 Then '插入“题注”对话框秀出来,如果按确定结束时执行以下程序，避免按取消后的空格,357也可换成wdDialogInsertCaption

      Application.ScreenUpdating = False     '关闭屏幕更新

       Lab = Dialogs(357).Label
       endPt = Selection.Start  'endPt标记插入的题注部分终点
      Selection.Start = startPt  '选定插入的整个题注

      '删除标签与编号间的空格（英文后的保留）
       With Selection.Find
          .Text = Lab & " "
          .Forward = True   'False=向上查找,(True=向下查找)
          .MatchWildcards = False '不使用通配符
          If Lab Like "*[0-9a-zA-Z.]" Then  '此处判断标签的最后一个字符是否为英文或数字，是则不删除空格
          Else
             .Replacement.Text = Lab
             .Execute Replace:=wdReplaceOne  '替换找到的第一个，此处用作删除空格
             endPt = endPt - 1 '删除空格后，末位减1
             Selection.End = endPt
          End If
       End With

      '在题注数字后添加一个空格
      Selection.Fields.ToggleShowCodes  '切换域代码，这样才能用^d查找域
       With Selection.Find
          .Text = "^d"
          .Replacement.Text = "^& "
          .Forward = False   'False=向上查找,(True=向下查找)
          .MatchWildcards = False '不使用通配符
          .Execute Replace:=wdReplaceOne  '替换找到的第一个，此处用作添加空格
       End With

      '选定整个插入的题注内容，将域代码切换回来
       endPt = endPt + 1 '增加空格后，末位加1
       With Selection
          .Start = startPt
          .End = endPt
          .Fields.ToggleShowCodes   '切换域代码（切换回来）
       End With

      '将光标定位至题注所在段尾处
'       Selection.MoveRight Unit:=wdCharacter, Count:=1  '此句光标返回插入题注前的原始位置，对于已经输好标题的情况并不合适
      '选择段尾回车符
       With Selection.Find
          .Text = "^13"
          .Forward = True   'False=向上查找,(True=向下查找)
          .MatchWildcards = False  '不使用通配符
          .Wrap = wdFindContinue '继续查找
          .Execute
       End With
      Selection.MoveLeft Unit:=wdCharacter, Count:=1  '定位到段尾回车前

   End If
   Application.ScreenUpdating = True          '恢复屏幕更新

End Sub
Sub 批量修改表格()
    Dim tempTable As Table
    Application.ScreenUpdating = False
    If ActiveDocument.ProtectionType = wdAllowOnlyFormFields Then
        MsgBox "文档已保护，此时不能选中多个表格！"
        Exit Sub
    End If
    ActiveDocument.DeleteAllEditableRanges wdEditorEveryone
    For Each tempTable In ActiveDocument.Tables
        tempTable.Range.Editors.Add wdEditorEveryone
    Next
    ActiveDocument.SelectAllEditableRanges wdEditorEveryone
    ActiveDocument.DeleteAllEditableRanges wdEditorEveryone
    Application.ScreenUpdating = True
End Sub
Sub FormatAllTables()
    For i = 1 To ActiveDocument.Tables.Count
       ' ActiveDocument.Tables(i).Style = "my"
        With ActiveDocument.Tables(i).Range.ParagraphFormat
        .LeftIndent = CentimetersToPoints(0)
        .RightIndent = CentimetersToPoints(0)
        .SpaceBefore = 0
        .SpaceBeforeAuto = False
        .SpaceAfter = 0
        .SpaceAfterAuto = False
        .LineSpacingRule = wdLineSpace1pt5
        .Alignment = wdAlignParagraphJustify
        .WidowControl = False
        .KeepWithNext = False
        .KeepTogether = False
        .PageBreakBefore = False
        .NoLineNumber = False
        .Hyphenation = True
        .FirstLineIndent = CentimetersToPoints(0)
        .OutlineLevel = wdOutlineLevelBodyText
        .CharacterUnitLeftIndent = 0
        .CharacterUnitRightIndent = 0
        .CharacterUnitFirstLineIndent = 0
        .LineUnitBefore = 0
        .LineUnitAfter = 0
        .MirrorIndents = False
        .TextboxTightWrap = wdTightNone
        .AutoAdjustRightIndent = True
        .DisableLineHeightGrid = False
        .FarEastLineBreakControl = True
        .WordWrap = True
        .HangingPunctuation = True
        .HalfWidthPunctuationOnTopOfLine = False
        .AddSpaceBetweenFarEastAndAlpha = True
        .AddSpaceBetweenFarEastAndDigit = True
        .BaseLineAlignment = wdBaselineAlignAuto
        End With
        ' 设置表中的字体及大小
        ActiveDocument.Tables(i).Select
         With Selection
         .Font.Size = 10
         .Font.Name = "宋体"
         .InsertCaption Label:="表格", TitleAutoText:="InsertCaption1", _
             Title:="", Position:=wdCaptionPositionAbove, ExcludeLabel:=0
        End With
        ActiveDocument.Tables(i).Cell(1, 1).Select
        With Selection
         .SelectRow
         .Font.Bold = True
         .Shading.BackgroundPatternColor = -603923969
         .ParagraphFormat.Alignment = wdAlignParagraphCenter
        End With
 Next
End Sub
Sub 表格题注()
'
'
    If ActiveDocument.Tables.Count >= 1 Then
        Set act_Doc = ActiveDocument
        For Each otable In act_Doc.Tables
            CaptionLabels.Add Name:="表星星星"
            With otable.Range.InsertCaption(Label:="表星星星", Position:=wdCaptionPositionAbove)
            'Position:=wdCaptionPositionBelow
            End With
        Next
    End If
 
End Sub

Sub 字体调整()
'
' 字体调整 宏
'
'
    Selection.Font.Name = "仿宋_GB2312"
    Selection.Font.Size = 14
    Selection.Font.Color = -587137025
End Sub
Sub 删除换行及空格()
'
' 删除换行及空格 宏
' 用于调整从PDF文件中复制出的文字格式
'
Selection.Find.ClearFormatting
Selection.Find.Replacement.ClearFormatting
With Selection.Find
    .Text = " "
    .Replacement.Text = ""
    .Forward = True
    .Wrap = wdFindStop
    .Format = False
    .MatchCase = False
    .MatchWholeWord = False
    .MatchByte = True
    .MatchWildcards = False
    .MatchSoundsLike = False
    .MatchAllWordForms = False
End With
Selection.Find.Execute Replace:=wdReplaceAll
Selection.Find.Replacement.ClearFormatting
With Selection.Find
    .Text = "^p^p"
    .Replacement.Text = "^p"
    .Forward = True
    .Wrap = wdFindStop
    .Format = False
    .MatchCase = False
    .MatchWholeWord = False
    .MatchByte = True
    .MatchWildcards = False
    .MatchSoundsLike = False
    .MatchAllWordForms = False
End With
Selection.Find.Execute Replace:=wdReplaceAll
End Sub
Sub 删除空白()
'
' 删除空白 宏
'
'
'    Selection.WholeStory
'
    Application.Run MacroName:="Normal.NewMacros.删除换行及空格"
    Application.Run MacroName:="Normal.NewMacros.首行缩进"
    Selection.Find.ClearFormatting
    Selection.Find.Replacement.ClearFormatting
    With Selection.Find
        .Text = "^p^p"
        .Replacement.Text = "^p"
        .Forward = True
        .Wrap = wdFindAsk
        .Format = False
        .MatchCase = False
        .MatchWholeWord = False
        .MatchByte = True
        .MatchWildcards = False
        .MatchSoundsLike = False
        .MatchAllWordForms = False
    End With
    Selection.Find.Execute Replace:=wdReplaceAll
End Sub
Sub 英文改times字体()
'
' 英文改times字体 宏
'
'
    Selection.Find.ClearFormatting
    Selection.Find.Replacement.ClearFormatting
    With Selection.Find
        .Text = "[0-9a-zA-Z]"
        .Replacement.Text = ""
        .Forward = True
        .Wrap = wdFindContinue
        .Format = True
        .MatchCase = False
        .MatchWholeWord = False
        .MatchByte = False
        .MatchAllWordForms = False
        .MatchSoundsLike = False
        .MatchWildcards = True
    End With
    Selection.Find.Execute Replace:=wdReplaceAll
End Sub

```