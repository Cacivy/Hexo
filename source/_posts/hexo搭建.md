---
title: 使用Hexo搭建Blog
date: 2016-06-13
---



Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

<!-- more -->

------


好了，以上是hexo默认的文档。

接下来，简单介绍下如何部署到github pages

<div class="tip">
<p>适用于以下版本</p>
<p>hexo: 3.2.0</p>
<p>hexo-cli: 1.0.2</p>
<p>os: Windows_NT 10.0.10586 win32 x64</p>
</div>

首先要添加SSH KEY，具体可以看[这里](http://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html)

<div class='tip'>
  注意在Enter passphrase for key这一步时，直接回车即可，如果输入passphrase的话hexo deploy可能会报错
</div>

之后配置_config.yml

``` json
deploy:
  type: git
  repository: git@github.com:Cacivy/Cacivy.github.io.git
  branch: master
```
<div class="tip">
<p>1. tpye后面需要加一个半角空格</p>
<p>2. 3.0版本后type需要从github改为git</p>
</div>

最后继续按文档来

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)

> SUCCESS
