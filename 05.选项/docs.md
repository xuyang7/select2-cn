---
title: Options 标题：选项
taxonomy: 分类
    category: docs 类别：docs
process:
    twig: true
never_cache_twig: true
---

传统的`<select>`框可包含任意数量`<option>`元素。每个元素都转换为下拉菜单中的一个下拉项。Select2 保留了这一特性，即在初始化包含`<option>`元素的`<select>`元素时，将其转换为其内部JSON格式：

```
{
  "id": "value attribute" || "option text",
  "text": "label attribute" || "option text",
  "element": HTMLOptionElement
}
```

`<optgroup>`元素将按以下规则被转换为数据目标：

```
{
  "text": "label attribute",
  "children": [ option data object, ... ],
  "element": HTMLOptGroupElement
}
```

>>> Options sourced from [other data sources](/data-sources) must conform to this this same internal representation.  See ["The Select2 data format"](/data-sources/formats) for details.源自[其他数据源](/data-sources)的选项必须符合此相同的内部表征。更多细节请参考["The Select2 数据格式"](/data-sources/formats) 。

## 下拉菜单组

HTML中`<option>`元素可以打包到一个`<optgroup>`元素中：

```
<select>
  <optgroup label="Group Name">
    <option>嵌入式下拉项</option>
  </optgroup>
</select>
```

Select2 会自动拾取这些元素并将其渲染到指定下拉菜单中。

### 分层下拉项

每个HTML规范只允许单层嵌套。如果将一个`<optgroup>`与另一个`<optgroup>`嵌套，Select2 无法检测到嵌套的另一层，从而导致错误触发。

此外，`<optgroup>`元素**不能**做成可选项。这是由于HTML规范的限制，而这个限制是Select2 无法解决的。

如果您确实需要创建一个分层的可选选项，用`<option>`代替`<optgroup>`并[用CSS 改变样式](http://stackoverflow.com/q/30820215/359284#30948247).

## 禁用选项

Select2 会正确处理禁用属性，无论是来自标准select数据(当设置 `disabled` 属性）还是来自其对象有着`disabled: true` 设置的远程数据源。

<div class="s2-example">
    <select class="js-example-disabled-results form-control">
      <option value="one">第一</option>
      <option value="two" disabled="disabled">第二 (禁用)</option>
      <option value="three">第三</option>
    </select>
</div>

<pre data-fill-from=".js-code-disabled-option"></pre>

```
<select class="js-example-disabled-results">
  <option value="one">第一</option>
  <option value="two" disabled="disabled">第二 (禁用)</option>
  <option value="three">第三</option>
</select>
```

<script type="text/javascript" class="js-code-disabled-option">

var $disabledResults = $(".js-example-disabled-results");
$disabledResults.select2();

</script>
