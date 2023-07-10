将Comment中的字符复制至Name中，判断name=code的情况下才复制，防止手动修改的内容被覆盖
``` script
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


```
2.将Name中的字符复制至Comment中，只有Comment字段为空才复制
``` script
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


```


将CODE列小写转换为大写(包含表名name和列name)

'大小写转换，目标为大写
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
添加默认字段

``` script
Option Explicit
ValidationMode = True
InteractiveMode = im_Batch
 

Dim mdl 
Set mdl = ActiveModel
If (mdl Is Nothing) Then
   MsgBox "There is no Active Model"
Else
   ListObjects(mdl)
End If
 
 

Private Sub ListObjects(fldr) 
   output "Scanning " & fldr.code
   Dim obj 
   For Each obj In fldr.children
      
      TableSetComment obj
   Next
   		
   Dim f 
   For Each f In fldr.Packages 
     
      ListObjects f
   Next
End Sub
 
Private Sub TableSetComment(CurrentObject)
   if not CurrentObject.Iskindof(cls_Table) then exit sub
   // 添加你需要排除的表
      if instr(",standard_data_field,",","+CurrentObject.Code+",")>0 then exit sub
      if not CurrentObject.isShortcut then
        
         Dim col  
         Dim num
         //定义你需要添加的字段；
         dim create_user_id_
         dim create_time_
         dim modify_user_id_
         dim modify_time_
         dim is_delete_
         dim is_final_
         dim tenant_id_
         dim team_id_
        //给字段定义一个初始值；
        create_user_id_=1
        modify_user_id_=1
        create_time_=1
        modify_time_=1
        is_delete_=1
		is_final_ = 1
		tenant_id_ = 1
		team_id_ = 1
        //循环获取到最大列号；
         for each col in CurrentObject.columns
            
              num= num + 1
       //如果列中有此字段，赋个其他值；（可能是，实际操作如果表中有此字段，会报错）
            if col.Code="create_user_id_" then
               create_user_id_=100
            end if
            
            if col.Code="create_time_" then
               create_time_=100
              
            end if
            
            if col.Code="modify_user_id_" then
               modify_user_id_=100
             
            end if
            
            if col.Code="modify_time_" then
               modify_time_=100
 
            end if
            
            if col.Code="is_delete_" then
               is_delete_=100
 
            end if
            
            
         next
         
		//下面的操作是进行赋默认值，根据需要进行设计即可。
         if create_user_id_=1 then 
         CurrentObject.columns.CreateNewAt(num)
            for each col in CurrentObject.columns 
          
            if col.DataType="" then 
                 	col.Name="创建人"
                 	col.Code="create_user_id_"
                 	col.Comment="创建人"
                 	col.DataType="bigint(20)"
					col.Mandatory=1
            end if 
            next
            num=num+1
         end if
         
         if create_time_=1 then 
         CurrentObject.columns.CreateNewAt(num)
            for each col in CurrentObject.columns 
           
            if col.DataType="" then 
                 	col.Name="创建时间"
                 	col.Code="create_time_"
                 	col.Comment="创建时间"
                 	col.DataType="datetime"
		col.DefaultValue="CURRENT_TIMESTAMP"
            end if 
            next
            num=num+1
         end if
         

         
         if modify_user_id_=1 then 
            CurrentObject.columns.CreateNewAt(num)
            for each col in CurrentObject.columns 
          
            if col.DataType="" then 
                 	col.Name="修改人"
                 	col.Code="modify_user_id"
                 	col.Comment="修改人"
                 	col.DataType="bigint(20)"     
            end if 
            next
            num=num+1
         end if
         
         
         if modify_time_=1 then 
            CurrentObject.columns.CreateNewAt(num)
            for each col in CurrentObject.columns 
          
            if col.DataType="" then 
                 	col.Name="修改时间"
                 	col.Code="modify_time_"
                 	col.Comment="修改时间"
                 	col.DataType="datetime"
		col.DefaultValue="CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP"
            end if 
            next
            num=num+1
         end if
         

		 if is_delete_=1 then 
            CurrentObject.columns.CreateNewAt(num)
            for each col in CurrentObject.columns 
          
            if col.DataType="" then 
                 	col.Name="删除标志"
                 	col.Code="is_delete_"
                 	col.Comment="删除标识(1:已删除;0:正常)"
                 	col.DataType="tinyint(1)"
		col.DefaultValue="0"
            end if 
            next
            num=num+1
         end if
				 
      if is_final_=1 then 
            CurrentObject.columns.CreateNewAt(num)
            for each col in CurrentObject.columns 
          
            if col.DataType="" then 
                 	col.Name="修改标志"
                 	col.Code="is_final_"
                 	col.Comment="是否不可修改(1:不可修改;0:可修改)"
                 	col.DataType="tinyint(1)"
		col.DefaultValue="0"
            end if 
            next
            num=num+1
         end if
         
      if tenant_id_=1 then 
            CurrentObject.columns.CreateNewAt(num)
            for each col in CurrentObject.columns 
          
            if col.DataType="" then 
                 	col.Name="租户id"
                 	col.Code="tenant_id_"
                 	col.Comment="租户id"
                 	col.DataType="bigint(20)"
		col.DefaultValue="-1"
            end if 
            next
            num=num+1
         end if
         
      if team_id_=1 then 
            CurrentObject.columns.CreateNewAt(num)
            for each col in CurrentObject.columns 
          
            if col.DataType="" then 
                 	col.Name="团队id"
                 	col.Code="team_id_"
                 	col.Comment="团队id"
                 	col.DataType="bigint(20)"
            end if 
            next
            num=num+1
         end if
         
      end if      
End Sub


```
