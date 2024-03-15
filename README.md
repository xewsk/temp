#!/bin/bash

# 递归查找并替换函数
replace_in_directory() {
    local directory="$1"
    local old_name="$2"
    local new_name="$3"
    local folder
    
    # 查找匹配的文件夹并进行替换
    find "$directory" -type d -name "*$old_name*" | while IFS= read -r folder; do
        # 计算新的文件夹路径
        new_folder=$(echo "$folder" | sed "s/$old_name/$new_name/")
        # 递归更新子文件夹名称
        replace_in_directory_recursive "$folder" "$old_name" "$new_name"
        # 重命名文件夹
        mv "$folder" "$new_folder"
        echo "Renamed: $folder -> $new_folder"
    done
}

# 递归更新子文件夹名称函数
replace_in_directory_recursive() {
    local directory="$1"
    local old_name="$2"
    local new_name="$3"
    local folder

    # 查找子文件夹并进行替换
    find "$directory" -mindepth 1 -type d | while IFS= read -r folder; do
        # 计算新的文件夹路径
        new_folder=$(echo "$folder" | sed "s/$old_name/$new_name/")
        # 重命名子文件夹
        mv "$folder" "$new_folder"
        echo "Renamed: $folder -> $new_folder"
        # 递归更新子文件夹名称
        replace_in_directory_recursive "$new_folder" "$old_name" "$new_name"
    done
}

# 指定目录和要替换的目录名
directory="/path/to/search"
old_name="old_folder_name"
new_name="new_folder_name"

# 调用函数进行递归查找并替换
replace_in_directory "$directory" "$old_name" "$new_name"




业务中台


#!/bin/bash

# 指定目录和要替换的目录名
directory="/path/to/search"
old_name="old_folder_name"
new_name="new_folder_name"

# 递归查找并替换
find "$directory" -type d -name "*$old_name*" -print0 | while IFS= read -r -d '' folder; do
    new_folder=$(echo "$folder" | sed "s/$old_name/$new_name/")
    mv "$folder" "$new_folder" && echo "Renamed: $folder -> $new_folder"
done
