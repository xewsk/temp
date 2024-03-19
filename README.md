#!/bin/bash

# 定义文件夹路径与GitLab仓库对应关系的列表
declare -A folder_repo_map
folder_repo_map["/path/to/folder1"]="git@gitlab.com/username/repo1.git"
folder_repo_map["/path/to/folder2"]="git@gitlab.com/username/repo2.git"
# 继续添加更多文件夹与仓库的映射

# 遍历映射关系，依次将文件夹上传到对应的GitLab仓库中
for folder in "${!folder_repo_map[@]}"; do
    repo="${folder_repo_map[$folder]}"
    echo "Uploading $folder to $repo"
    
    # 切换到临时目录，准备上传文件夹内容到GitLab仓库
    tmp_dir=$(mktemp -d)
    cp -r "$folder"/* "$tmp_dir" # 将文件夹内容复制到临时目录
    cd "$tmp_dir"
    
    # 初始化Git仓库并提交文件夹内容
    git init
    git add .
    git commit -m "Initial commit"
    
    # 添加GitLab远程仓库并上传内容
    git remote add origin "$repo"
    git push -u origin master
    
    # 清理临时目录
    cd ..
    rm -rf "$tmp_dir"
done
