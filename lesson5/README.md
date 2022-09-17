# Git清理commit中历史提交的大文件

## 查看目录下的文件的大小
```
git count-objects -v # 查看 git 相关文件占用的空间
du -sh .git # 查看 .git 文件夹占用磁盘空间
du -d 1 -h  # 列出所有文件的大小
```

## 找到大文件
```
git rev-list --all | xargs -rL1 git ls-tree -r --long | sort -uk3 | sort -rnk4 | head -10

```

##  重写commit，删除大文件

```
git filter-branch --force --index-filter 'git rm -rf --cached --ignore-unmatch big-file.jar' --prune-empty --tag-name-filter cat -- --all
```


## 推送修改后的repo

```
git push origin master --force
git push origin --force --all
```


## 清理和回收空间

```
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now
```
