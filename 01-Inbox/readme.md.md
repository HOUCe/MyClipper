# readme.md

[github.com](https://github.com/flowfire/demo003)flowfire

测试链接： [http://128.199.71.75:9999/](http://128.199.71.75:9999/)-
注：该 demo 仅支持使用纯英文搜索，其他语言可能会出现意外的 bug

开发思路

1.  经询问，确认无需实现复杂搜索系统，页面关系也可自由发挥
2.  首先考虑 Google Search api 和 解析 wikipedia 页面
3.  经搜索后发现，维基百科自带 api 文档，其中，搜索功能可直接跨域使用，页面详情仅站内可用
4.  不能仅开发纯前端内容，需要后端配合

因此决定采用我之前写过的一个支持 SPA 应用的小型后端服务器-
前端采用 echarts 展示属性图，点击子节点或搜索时都会自动查询关键字并把关键字作为父节点-
子节点的内容采用维基百科返回的站内链接，取其前十位

本地启动办法：

1.  环境配置： node lts 版本， git 最新版本。 机器可直连维基百科或 terminal 中配置了 proxy 环境变量
2.  clone 本 repo 到任意文件夹
3.  cd 到该文件夹，并依次执行下列命令
4.  `npm i -g fffmwk`
5.  `fff`
6.  打开 localhost:9999 查看效果

[查看原网页: github.com](https://github.com/flowfire/demo003)