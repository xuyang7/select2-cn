---
标题：选项
分类：
    目录：docs
process:
    twig: true
never_cache_twig: true
---

当从下拉菜单中完成选择后，Select2会在容器框内显示当前
选择值。默认情况下，会显示Select2[选项的内部表示](/options)的`text`属性。

## 模板

使用`templateSelection`配置选项，可以自定义选择结果的外观。这需要一个回调函数将选择数据对象转换成字符串或jQuery对象：

<div class="s2-example">
    <select class="js-example-templating js-states form-control"></select>
</div>

<pre data-fill-from=".js-code-example-templating"></pre>

<script type="text/javascript" class="js-code-example-templating">

function formatState (state) {
  if (!state.id) {
    return state.text;
  }
  var baseUrl = "{{ url('user://pages/images/flags') }}";
  var $state = $(
    '<span><img src="' + baseUrl + '/' + state.element.value.toLowerCase() + '.png" class="img-flag" /> ' + state.text + '</span>'
  );
  return $state;
};

$(".js-example-templating").select2({
  templateSelection: formatState
});

</script>

>>>使用如[Handlebars](http://handlebarsjs.com/)类似的客户端模板引擎来定义您的模板是很有帮助的。

### 内置转义

默认情况下，`templateResult`返回的字符串**只包含文本**，并且会被转交至`escapeMarkup`函数，该函数会清除所有HTML标记。

如果您需要用选择模板渲染HTML，则必须将您呈现的结果封装到一个jQuery对象中。在这种情况下，选项会被传递至[指向`jQuery.fn.append`](https://api.jquery.com/append/)并由jQuery直接处理。任何标记，比如HTML，都不会被转义。您可以有选择的将用户提供的恶意输入进行转义。

>>>> 任何通过选项而渲染的都是模板化的结果。这包括占位符及所显示的预置选项，所以您必须确保您的模板化功能可以支持它们。

## 选项数量限制

 Select2多值选择框可以对选项的最大数量设置限制。下面的选择通过在Select2选项里`multiple`属性的`maximumSelectionLength` 来声明。

<div class="s2-example">
    <p>
      <select class="js-example-basic-multiple-limit js-states form-control" multiple="multiple"></select>
    </p>
</div>

<pre data-fill-from=".js-code-placeholder"></pre>

<script type="text/javascript" class="js-code-placeholder">

$(".js-example-basic-multiple-limit").select2({
  maximumSelectionLength: 2
});

</script>

## 选项清除

当设置为`true`，在选择了一个值时，会有一个清除按钮（"x" 图标）出现在选择框上。点击这个按钮将清除所选择的值，有效地将选择框重置回其占位符值。

```
$('select').select2({
  placeholder: 'This is my placeholder',
  allowClear: true
});
```
