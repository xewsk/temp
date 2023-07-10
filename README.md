将Comment中的字符复制至Name中，判断name=code的情况下才复制，防止手动修改的内容被覆盖
Option   Explicit 
ValidationMode   =   True 
InteractiveMode   =   im_Batch

Dim   mdl   '   the   current   model

'   get   the   current   active   model 
Set   mdl   =   ActiveModel 
If   (mdl   Is   Nothing)   Then 
      MsgBox   "There   is   no   current   Model " 
ElseIf   Not   mdl.IsKindOf(PdPDM.cls_Model)   Then 
      MsgBox   "The   current   model   is   not   an   Physical   Data   model. " 
Else 
      ProcessFolder   mdl 
End   If

Private   sub   ProcessFolder(folder) 
On Error Resume Next
      Dim   Tab   'running     table 
      for   each   Tab   in   folder.tables 
            if   not   tab.isShortcut   then
				if tab.name=tab.code then
                  tab.name   =   tab.comment
				end if
                  Dim   col   '   running   column 
                  for   each   col   in   tab.columns 
                  if col.comment="" then
                  else
					  if col.name=col.code then
                        col.name=   col.comment
					  end if
                  end if
                  next 
            end   if 
      next

      Dim   view   'running   view 
      for   each   view   in   folder.Views 
            if   not   view.isShortcut   then 
			    if view.name=view.code then
                  view.name   =   view.comment 
				end if
            end   if 
      next

      '   go   into   the   sub-packages 
      Dim   f   '   running   folder 
      For   Each   f   In   folder.Packages 
            if   not   f.IsShortcut   then 
                  ProcessFolder   f 
            end   if 
      Next 
end   sub
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
2.将Name中的字符复制至Comment中，只有Comment字段为空才复制
Option   Explicit 
ValidationMode   =   True 
InteractiveMode   =   im_Batch

Dim   mdl   '   the   current   model

'   get   the   current   active   model 
Set   mdl   =   ActiveModel 
If   (mdl   Is   Nothing)   Then 
      MsgBox   "There   is   no   current   Model " 
ElseIf   Not   mdl.IsKindOf(PdPDM.cls_Model)   Then 
      MsgBox   "The   current   model   is   not   an   Physical   Data   model. " 
Else 
      ProcessFolder   mdl 
End   If

'   This   routine   copy   name   into   comment   for   each   table,   each   column   and   each   view 
'   of   the   current   folder 
Private   sub   ProcessFolder(folder) 
      Dim   Tab   'running     table 
      for   each   Tab   in   folder.tables 
            if   not   tab.isShortcut  then 
				if tab.comment="" then
                  tab.comment   =   tab.name 
				end if
                  Dim   col   '   running   column 
                  for   each   col   in   tab.columns 
					if col.comment="" then
                        col.comment=   col.name 
					end if
                  next 
            end   if 
      next

      Dim   view   'running   view 
      for   each   view   in   folder.Views 
            if   not   view.isShortcut   then 
				if view.comment="" then
                  view.comment   =   view.name
				end if
            end   if 
      next

      '   go   into   the   sub-packages 
      Dim   f   '   running   folder 
      For   Each   f   In   folder.Packages 
            if   not   f.IsShortcut   then 
                  ProcessFolder   f 
            end   if 
      Next 
end   sub


————————————————
版权声明：本文为CSDN博主「-luking-」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_44565095/article/details/113614497


将CODE列小写转换为大写(包含表名name和列name)

'大小写转换，目标为大写
Option Explicit
ValidationMode = True
InteractiveMode = im_Batch
Dim mdl ' the current model
'取得当前Model
Set mdl = ActiveModel
If (mdl Is Nothing) Then
 MsgBox "There is no current Model"
ElseIf Not mdl.IsKindOf(PdPDM.cls_Model) Then
 MsgBox "The current model is not an Physical Data model."
Else
 ProcessFolder mdl
End If
 
Private sub ProcessFolder(folder)
  '处理表
 Dim Tab
 for each Tab in folder.tables
   tab.code = UCase(tab.code)
   '修改字段名
   Dim col
   for each col in tab.columns
    col.code= UCase(col.code)
   next
   '修改索引名
   Dim idx
   for each idx in tab.indexes
    idx.code= UCase(idx.code)
   next
   '修改主键名
   Dim key
   for each key in tab.keys
    key.code= UCase(key.code)
   next
 next
' 同理处理视图
' Dim view
' for each view in folder.Views
 ' if not view.isShortcut then
   ' view.code = view.name
  ' end if
' next
 ' go into the sub-packages
 Dim f ' running folder
 For Each f In folder.Packages
  if not f.IsShortcut then
   ProcessFolder f
  end if
 Next
end sub
