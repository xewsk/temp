#!/bin/bash

# 定义递归函数
rename_folders() {
    local directory="$1"
    local old_name="$2"
    local new_name="$3"
    
    # 处理当前目录
    for folder in "$directory"/*/; do
        if [ -d "$folder" ] && [[ "$folder" == *"$old_name"* ]]; then
            new_folder="${folder//$old_name/$new_name}"
            mv "$folder" "$new_folder" && echo "Renamed: $folder -> $new_folder"
        fi
    done
    
    # 递归处理子目录
    for folder in "$directory"/*/; do
        if [ -d "$folder" ]; then
            rename_folders "$folder" "$old_name" "$new_name"
        fi
    done
}

# 调用递归函数
rename_folders "/path/to/search" "old_folder_name" "new_folder_name"
