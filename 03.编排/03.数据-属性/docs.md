---
标题: data-* 属性
分类:
    目录: docs
---

建议通过将配置选项以[对象传入](/configuration)方式初始化Select2。不过您也可以使用 HTML5 中的 `data-*` 属性来定义选项默认值，该属性将覆盖通过Select2 初始化时候设置的值和选项的任何[默认值](/configuration/defaults)。

```
<select data-placeholder="Select a state">
  <option value="AL">Alabama</option>
    ...
  <option value="WY">Wyoming</option>
</select>
```

## 嵌套的（子项）选项

Sometimes, you have options that are nested under a top-level option.  For example, the options under the `ajax` option: 有时在顶层选项下会有嵌套的子选项。比如，在`ajax`选项下的子项。

```
$(".js-example-data-ajax").select2({
  ajax: {
    url: "http://example.org/api/test",
    cache: false
  }
});
```

将这些选项写成`data-*`属性，每个级别的嵌套应由两个破折号(`--`)分开。

```
<select data-ajax--url="http://example.org/api/test" data-ajax--cache="true">
    ...
</select>
```

该选项值取决于jQuery'的 HTML5数据属性的[解析规则](https://api.jquery.com/data/#data-html5)

>>> 由于[一个jQuery漏洞](https://github.com/jquery/jquery/issues/2070)，[在jQuery.1.x下](https://github.com/select2/select2/issues/2969)使用`data-*`属性，无法获得嵌套选项。

## `camelCase` 选项
 
HTML数据属性不区分大小写，所以任何包含大写字母的选项会被解析为全部是小写。因为Select2有很多驼峰大小写的选项，其语句由大写字母区分，您必须将这些选项用破折号来代替。所以正常命名为`allowClear`的选项应写成`allow-clear`.

这意味着将您的`<select>`标签声明为...

```
<select data-tags="true" data-placeholder="Select an option" data-allow-clear="true">
    ...
</select>
```
 
 效果同Select2初始化配置...

```
$("select").select2({
  tags: "true",
  placeholder: "Select an option",
  allowClear: true
});
```
