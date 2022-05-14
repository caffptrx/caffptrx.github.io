---
title: Search Icon 问题
date: 2022-05-14 11:59:55
tags:
    - Hexo
    - NexT
categories:
    - 开发
---
# 问题描述
在使用 Local Search 时，搜索图标会遇到这种问题
{% asset_img prob-pic.png 一直转圈的图标%}
# 解决办法
在博客根目录下的 `_config.yml` 修改配置
```
search:
  path: search.json // 官方教程的文件名为 search.xml，这就是错误点
  field: post
  format: html
  limit: 10000
```
# 测试
```
hexo clean
hexo g
hexo s
```