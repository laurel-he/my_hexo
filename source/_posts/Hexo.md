---
title: 在github上使用Hexo 搭建个人博客
date: 2019-04-01 11:03:10
tags:
categories: 前端
---
# 安装
**简介**
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
**安装前提**
node.js
git
**安装步骤**
```
npm install -g hexo-cli
```
# 建站
```
hexo init <folder>
cd <folder>
npm install
```
# 创建博客
```
hexo new [layout] <title> #hexo n
hexo generate（或者hexo g:生成静态文件） #hexo g
hexo publish [layout] <filename>（发布草稿）
hexo server（启动服务器）
hexo deploy（或者hexo d:部署）
hexo render <file1> [file2] ...（渲染文件）
hexo migrate <type>（从其他博客迁移内容）
hexo clean（清除缓存文件 (db.json) 和已生成的静态文件 (public)）
hexo list（列出网站资料）
hexo version（显示 Hexo 版本）
#部署使用hexo deploy (hexo d,需要先编译)
```
# 添加评论
可以使用gitment或者valine
[valine评论](https://blog.csdn.net/blue_zy/article/details/79071414 "valine评论")
[gitment评论](https://blog.csdn.net/Biebersxzl/article/details/81913680 "gitment评论")
**注意事项**
1 如果添加了CNAME，回调地址需要填绑定的地址
2 如果需要开启评论，除了百度能搜到的步骤以外，还需要本地安装博客评论模块
npm i --save gitment
再重新hexo g -d一下
3 作者的服务器停掉了，因此需要修改js源码来源：
[修改源](https://blog.csdn.net/wardseptember/article/details/82828391 "修改源")
（也可以wget将源码下载下来后修改源到自己服务器上）





