---
layout: mypost
title: 使用 Power Automate 工作流自动合并文件夹中的 PDF 文件
categories: [PowerAutomate]
---

最近开始使用 Power Automate 发现真的非常好用。

下面是一个简单的 PDF 合并的工作流，比之前使用 PDF 工具的合并操作方便了太多。

```
Display.SelectFolder Description: $'''请选择要做合并处理的 PDF 文件所在的文件夹''' InitialDirectory: $'''D:\\OneDrive\\80_Machine_Data\\''' IsTopMost: True SelectedFolder=> SelectedFolder ButtonPressed=> ButtonPressed

IF ButtonPressed = $'''Cancel''' THEN
EXIT Code: 0
END

IF (File.IfFile.Exists File: $'''%SelectedFolder%\\合并.PDF''') THEN
File.Delete Files: $'''%SelectedFolder%\\合并.PDF'''
END

Folder.GetFiles Folder: SelectedFolder FileFilter: $'''*.PDF''' IncludeSubfolders: False FailOnAccessDenied: True SortBy1: Folder.SortBy.FullName SortDescending1: True SortBy2: Folder.SortBy.NoSort SortDescending2: False SortBy3: Folder.SortBy.NoSort SortDescending3: False Files=> Files

Pdf.MergeFiles PDFFiles: Files MergedPDFPath: $'''%SelectedFolder%\\合并.PDF''' IfFileExists: Pdf.IfFileExists.AddSequentialSuffix PasswordDelimiter: $''',''' MergedPDF=> MergedPDF

Display.ShowMessageDialog.ShowMessage Title: $'''PDF 合并工作流完成。''' Message: $'''合并后的文件存储在：
%MergedPDF%
现在将打开合并后的文件浏览。''' Icon: Display.Icon.Information Buttons: Display.Buttons.OK DefaultButton: Display.DefaultButton.Button1 IsTopMost: True

System.RunApplication.RunApplication ApplicationPath: MergedPDF WindowStyle: System.ProcessWindowStyle.Normal

```

![image](PowerAutomate1.png)

说明：

如果要将 Power Automate 工作流复制出来，需要将每个 子流 复制并粘贴到记事本。

想要复制上面的工作流使用，也是一样的，每次仅能复制一个子流，粘贴到 Power Automate 里面。
