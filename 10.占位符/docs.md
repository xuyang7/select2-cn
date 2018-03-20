---
标题：占位符
分类：
    目录：docs
process:
    twig: true
never_cache_twig: true
---

Select2支持使用`placeholder`配置选项来显示占位符值。直到做出选择后占位符值才会消失。

## 文本占位符

最常见的情况是使用文本字符串作为您的占位符值。

### 单选框占位符

<div class="s2-example">
  <p>
    <select class="js-example-placeholder-single js-states form-control">
      <option></option>
    </select>
  </p>
</div>

```html
<select class="js-example-placeholder-single js-states form-control">
  <option></option>
</select>
```

<pre data-fill-from="#example-placeholder-single-select"></pre>

<script type="text/javascript" id="example-placeholder-single-select" class="js-code-placeholder">
$(".js-example-placeholder-single").select2({
    placeholder: "Select a state",
    allowClear: true
});
</script>

>>>**仅适于单选框**，为了出现占位符值，您必须要空白`<option>`作为`<select>` 的第一个选项。这是因为浏览器默认选择第一个选项。如果第一个选项为非空，浏览器会显示它而非占位符。

### 多选框占位符

对于多选框，则不允许有空的`<option>`元素：

<select class="js-example-placeholder-multiple js-states form-control" multiple="multiple"></select>

```html
<select class="js-example-placeholder-multiple js-states form-control" multiple="multiple"></select>
```

<pre data-fill-from="#example-placeholder-multi-select"></pre>

<script type="text/javascript" id="example-placeholder-multi-select" class="js-code-placeholder">
$(".js-example-placeholder-multiple").select2({
    placeholder: "Select a state"
});
</script>

>>> Select2多选框使用`placeholder`属性，这需要IE10以上的版本。在旧版本中使用可参照[the Placeholders.js polyfill](https://github.com/jamesallardice/Placeholders.js)。

## 默认选择占位符

或者，`placeholder`值选项可以是一个代表默认选择(`<option>`)的数据对象。这种情况下，该数据对象的`id`可以匹配对应选择的`value`。

```
$('select').select2({
  placeholder: {
    id: '-1', // 选项值
    text: 'Select an option'
  }
});
```
这很有用，比如当您使用框架创建其自身占位符选项时。

## 以AJAX方式使用占位符

Select2支持各种形式配置占位符，包括AJAX。当您使用单选框时，仍然需要增加空`<option>`。

##  自定义占位符外观

当使用Select2**单选模式**，如果指定了`templateSelection`回调函数，Select2会将占位符选项作为参数传递给`templateSelection`指定的回调函数。 您可以在该回调函数中用一些额外的逻辑来检查`id`属性并给您的占位符选项应用一个二选一的转换：

```
$('select').select2({
  templateSelection: function (data) {
    if (data.id === '') { // 调整为自定义的占位符值
      return 'Custom styled placeholder text';
    }

    return data.text;
  }
});
```

>>>>> **当允许多选时**，在搜索框中设置`placeholder`属性可以显示占位符。您可以用CSS来自定义该占位符的显示，下面的Stack Overflow 网站对此作了解释：[Change an input's HTML5 placeholder color with CSS](http://stackoverflow.com/q/2610497/359284)。

## 老版本Internet Explorer中的占位符

 Select2使用输入框的本地`placeholder` 属性实现多选框逻辑，而旧版本的Internet Explorer不支持该属性。您应使页面包含[Placeholders.js](https://github.com/jamesallardice/Placeholders.js) ，或者使用完全构建，从而使输入框增加`placeholder`属性支持。
