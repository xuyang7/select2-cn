---
title: Arrays
taxonomy:
    category: docs
process:
    twig: true
never_cache_twig: true
---

## 加载数组数据

您可以使用`data`配置选项来从本地数组中加载下拉选择项。

您可以通过为所选的值提供选项标签来提供初始选择，这与标准选择的方法类似。

<div class="s2-example">
  <p>
    <select class="js-example-data-array form-control"></select>
  </p>
  <p>
    <select class="js-example-data-array-selected form-control">
      <option value="2" selected="selected">duplicate</option>
    </select>
  </p>
</div>

<pre data-fill-from=".js-code-placeholder"></pre>

<script type="text/javascript" class="js-code-placeholder">

var data = [
    {
        id: 0,
        text: 'enhancement'
    },
    {
        id: 1,
        text: 'bug'
    },
    {
        id: 2,
        text: 'duplicate'
    },
    {
        id: 3,
        text: 'invalid'
    },
    {
        id: 4,
        text: 'wontfix'
    }
];

$(".js-example-data-array").select2({
  data: data
})

$(".js-example-data-array-selected").select2({
  data: data
})
</script>

不像这个[AJAX数据源 sources](/data-sources/ajax)提供下拉项的例子，作为数组提供的菜单项会很快。

##  与`tags`选项的向后兼容性

在Select3,5版本中，该选项叫做`tags`，但是在4.0版本中，`tags`用于是处理 [tagging 功能](/tagging)。

对于后向兼容性，`tags`选项仍可以以接收对象数组，在这种情况下，它们会被以`data`选项相同的方式被处理。