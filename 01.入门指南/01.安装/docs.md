---
title: Installation 标题：安装
taxonomy: 分类
    category: docs 目录：docs
---

您必须在您的网站上加载编译好的JavaScript和CSS文件才能使用Select2。在您的网站或应用里，加载这些预编译的文件（俗称**distribution**）有多种方法。

## 从CDN使用Select2

CDN,即内容运输网络，是建立和运行Select2的最快方式。

Select2在 [cdnjs](https://cdnjs.com/libraries/select2) 和 [jsDelivr](https://www.jsdelivr.com/#!select2) CDNs上都有版本提供，简单地在页面的`<head>`部分里加载以下几行代码。

```
<link href="https://cdnjs.cloudflare.com/ajax/libs/select2/4.0.6-rc.0/css/select2.min.css" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/select2/4.0.6-rc.0/js/select2.min.js"></script>
```

>>> <i class="fa fa-info-circle"></i>立即跟随一个新的发布，CDNs花费一些时间来获取CDN依赖的最新版本。

## 使用Bower安装

Select2可以在Bower上获取。将下面的代码增加到你的`bower.json`文件然后运行`bower install`：

```
"dependencies": {
    "select2": "~4.0"
}
```

或者，在你的项目目录运行`bower install select2`

预编译的分布文件可以在`vendor/select2/dist/css/`和`vendor/select2/dist/js/`里获取，相对于您的项目目录。在你的页面里引入：

```
<link href="vendor/select2/dist/css/select2.min.css" rel="stylesheet" />
<script src="vendor/select2/dist/js/select2.min.js"></script>
```

## 手动安装

强烈推荐您使用CDN或者类似Bower或npm的包裹管理器。这能让您在不同的环境中部署程序变得更加简单，并且当Select2新版本发布时，可以更简单地更新升级。虽然如此，如果您更愿意手动将Select2整合到您的程序中，您可以从Github[下载你需要的版本](https://github.com/select2/select2/tags)并从`dist`目录中复制文件到您的项目中。

引入编译过的文件到您的页面里：

```
<link href="path/to/select2.min.css" rel="stylesheet" />
<script src="path/to/select2.min.js"></script>
```
