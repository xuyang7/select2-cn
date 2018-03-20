---
title: Dropdown 标题：下拉
taxonomy: 分类：
    category: docs 目录：docs
process:
    twig: true
never_cache_twig: true
---

本章节阐述了下拉菜单清单的外观和操作。

## Templating 模板

Select2会默认显示结果集内每个数据对象的`text`属性。下拉菜单中搜索结果的外观可以用`templateResult`选项进行自定义：

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
  templateResult: formatState
});

</script>

 `templateResult`功能会返回一个字符串，该字符串包含要显示的文本或一个包含应显示数据的对象（比如jQuery对象）：

>>>  用客户端模板引擎来定义您的模板是很有帮助的，比如[Handlebars](http://handlebarsjs.com/)。

### 内置码

 默认情况下，`templateResult`返回的字符串**只包含文本**，并且会传递至`escapeMarkup`功能，该功能会清除所有HTML标记。

如果您需要用结果模板呈现HTML，则必须将您呈现的结果封装到一个jQuery对象中。在这种情况下，结果会被传递至[直接跳至 `jQuery.fn.append`](https://api.jquery.com/append/)并由jQuery直接处理。任何标记，比如HTML，都不会被转义。您可以自主将用户提供的恶意输入进行转义。

>>>  **所有被渲染对象都是模板化的**这包括比如"搜索..."和"加载更多"等文本，将会周期性地呈现，允许您给这些自动生成的选项增加更多高级格式。但您必须确保您的模板功能可以支持。

## 自动选择

当使用`selectOnClose`选项关闭下拉菜单时，Select2可以配置将当前自动选择的结果高亮展示。

```
$('#mySelect2').select2({
  selectOnClose: true
});
```

## 强制下拉菜单在选择接手后，仍保持打开状态

默认情况下数据选择后，Select2会自动关闭下拉框，与一般的选择框类似。你可以使用`closeOnSelect`选项来控制结果选择后是否关闭下拉菜单。

```
$('#mySelect2').select2({
  closeOnSelect: false
});
```

注意此选项仅适用于多选框。

>>>  如果[`CloseOnSelect` 参数](/advanced/default-adapters/dropdown#closeonselect)未使用，（或`closeOnSelect` 被设置成 <code>false</code>），当选择一个结果时，下拉框不会自动关闭。如果按着<kbd>ctrl</kbd>键，下拉框也不会关闭。

## 下拉框位置

>>>>>  注意[Harvest Chosen](https://harvesthq.github.io/chosen/)移植器。如果您正从Chosen移植到Select2，此选项会使Select2用相似的方式展示下拉框。

默认下，Select2会将下拉框附到正文的末尾并使其出现在待选项上方或下方。

如果选择框下方没有足够空间，而选择框上方有足够的空间，Select2会将下拉框显示在选择框的上方。

`dropdownParent` 选项允许您为下拉框选择一个附加的替代元素：

```
$('#mySelect2').select2({
  dropdownParent: $('#myModal')
});
```

当试图在模态框或其他小集合内成功渲染Select2时，就很有用了。比如当您在模态框内使用搜索框时遇到了问题，可以尝试将`dropdownParent`选项附着在模态框上。

 如果您在使用默认`body`附件时遇到位置问题，用浏览器控制台检验以下数值就会很有帮助：

- `document.body.style.position`
- `$(document.body).offset()`

参考[这个案例](https://github.com/select2/select2/issues/3970#issuecomment-160496724)

>>>> 由于`dropdownParent` 会将DOM事件转移至标准Select2 DOM容器之外，这将导致像modals一样的第三方插件出现问题。
