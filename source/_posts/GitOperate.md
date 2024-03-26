---

title: git 一些实用操作
date: 2022-04-26 10:34:39
tags: git

---

日常开发中 git 是经常用到的，下面记录一写实际会用到的 git 常用操作

<!-- more -->

## 回退 master 提交

```
  git checkout master // 切换至master
  git checkout -b master-backup  // 备份master
  git push origin master-backup // 将备份推送到origin
  git checkout master // 切换至master
  git reset [HEAD^1 | 052e($commit id)]   // reset上一次或指定提交记录
  git checkout . // 丢弃本地的修改
  git push --delete origin master // 将origin master删除
  git push origin master // 将本地master推到origin
```

## 查询一个月内某个人的代码提交行数

输出的样式类似 `added lines: 1659, removed lines: 450, total lines: 1209`

```
git log --author="name" --pretty=tformat: --numstat --since='1 month ago'  | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }'
```

## 查询提交前五的人
```
git log --pretty='%aN' | sort | uniq -c | sort -k1 -n -r | head -n 5
```

## 合并多条 commit

```
git rebase -i HEAD~3  // 合并最近3条commit
```

## merge 的时候合并合并多条 commit 为 1 条

```
git merge --squash {commit id}
```
