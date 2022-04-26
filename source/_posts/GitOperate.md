---
title: git一些实用操作
date: 2022-04-26 10:34:39
tags: git
---

日常开发中 git 是经常用到的，下面记录一写实际会用到的 git 常用操作

<!-- more -->

## 回退 master 合入 dev 代码

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
