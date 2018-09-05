---
title: hexo同时上传管理源码跟生成的网页
date: 2018-08-31 10:14:20
tags:
---

hexo d的时候，默认只是上传了public（可以手动在参数里面添加文件夹）里面生成的文件，而没有上传源码，如果需要在其他地方也写文章的话，就需要上传源码。
可以将源码上传到另外的一个repo里面，或者创建一个分支，单独上传一个分支。这里使用单独创建分支的方法。

```bash
#创建一个新的分支
git checkout -b source

#写一个新的文章
hexo new newpost
git add newpost
git commit -m "newpost add"

#生成并上传文章 默认动作 HEAD -> master
hexo g
hexo d

#上传源码 source->source
git push origin source
```

就可以同时上传source跟master到一个repo，方便管理