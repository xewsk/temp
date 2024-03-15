#!/bin/bash

# 指定目录和要替换的目录名
directory="/path/to/search"
old_name="old_folder_name"
new_name="new_folder_name"

# 递归查找并替换
find "$directory" -type d -name "*$old_name*" -print0 | while IFS= read -r -d '' folder; do
    # 获取最后一级目录名
    base_folder=$(basename "$folder")
    # 如果最后一级目录名与旧名称相同，则进行替换
    if [ "$base_folder" == "$old_name" ]; then
        # 构建新目录路径
        new_folder=$(dirname "$folder")/"$new_name"
        # 进行替换
        mv "$folder" "$new_folder" && echo "Renamed: $folder -> $new_folder"
    fi
done
