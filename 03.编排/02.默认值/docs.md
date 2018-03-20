---
标题: 全局默认值
分类:
    目录: docs
---

在某些情况下，您需要为web应用程序中的所有Select2实例设置默认选项。当您从Select2老版本迁移，或者使用类似[AMD构建](/getting-started/builds-and-modules#using-select2-with-amd-or-commonjs-loaders)这种非标准选项时，这一点特别有用。Select2通过`$.fn. Select2 .defaults`导出默认选项，这允许您将它们设置为全局变量。

在设置全局选项时，过去设置的任何默认值都将被重写，只有在初始化期间为设置选项时才会使用默认选项。

可通过调用 `$.fn.select2.defaults.set("key", "value")`设置选项默认值。 例如:

```
$.fn.select2.defaults.set("theme", "classic");
```

## 嵌套的选项

若需要为缓存设置默认值，请使用同[HTML `data-*` 属性](/configuration/data-attributes)一样的标记。2个破折号(--) 将被一个嵌套级别替换，一个破折号(-)将被转换为驼峰式字符串：

```
$.fn.select2.defaults.set("ajax--cache", false);
```

## 重置默认选项初始值

您可以通过如下调用来重置默认选项的初始值。

```
$.fn.select2.defaults.reset();
```
