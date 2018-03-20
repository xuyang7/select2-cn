---
标题：搜索
分类：
    目录：docs
process:
    twig: true
never_cache_twig: true
---

在下拉菜单的顶部自动为选择框增加了一个搜索框，这里只支持单选框。Select2可轻松自定义该搜索框的行为和外观。

## 自定义结果匹配程度

当用户在搜索框内输入搜索条目筛选结果时，Select2使用内部匹配器来将结果和搜索条目进行匹配。您可以通过为`matcher` 配置选项定义回调函数来自定义Select2匹配搜索条目的方式。

Select2会以[内部表示](/options)将每个选项传递至该回调函数来决定是否进行显示：

```
function matchCustom(params, data) {
    // 如果没有检索词，则返回所有数据
    if ($.trim(params.term) === '') {
      return data;
    }

    // 如果没有定义text字段，则不显示
    if (typeof data.text === 'undefined') {
      return null;
    }

    // `params.term` 表示指定的检索词
    // `data.text` 表示该数据对象的将显示什么文本
    if (data.text.indexOf(params.term) > -1) {
      var modifiedData = $.extend({}, data, true);
      modifiedData.text += ' (matched)';
   
      // 你可以返回修改后的数据结构
      // 包含在嵌套数据集中匹配你想要的解决
      return modifiedData;
    }
   
    // 如果检索词不需要展示，则返回`null` 
    return null;
}
    
$(".js-example-matcher").select2({
  matcher: matchCustom
});
```

>>>> `matcher`仅对 **本地提供数据**有效（比如，通过[array](/data-sources/arrays)!），当使用远程数据时，Select2会认为返回的结果已经在服务区端进行了筛选。

### 匹配组选项

只有第一级对象会被传递至`matcher`回调。如果您正在处理嵌入数据，则必须通过`children`排列对它们进行单独匹配。这就允许了处理嵌入对象时更高级的匹配，允许您任意进行处理。

这个例子仅匹配出现在字符串前缀匹配的结果：

<div class="s2-example">
    <select class="js-example-matcher-start js-states form-control"></select>
</div>

<pre data-fill-from=".js-code-example-matcher"></pre>

<script type="text/javascript" class="js-code-example-matcher">

function matchStart(params, data) {
  // 如果没有检索词则返回原始数据
  if ($.trim(params.term) === '') {
    return data;
  }

  // 忽略，如果没有'childer'属性
  if (typeof data.children === 'undefined') {
    return null;
  }

  // `data.children` 包括实际匹配的选项
  var filteredChildren = [];
  $.each(data.children, function (idx, child) {
    if (child.text.toUpperCase().indexOf(params.term.toUpperCase()) == 0) {
      filteredChildren.push(child);
    }
  });

  // If we matched any of the timezone group's children, then set the matched children on the group
  // 如果与任何时间组子对象匹配，则设置匹配的子组并返回组对象
  if (filteredChildren.length) {
    var modifiedData = $.extend({}, data, true);
    modifiedData.children = filteredChildren;

    // 这里可以返回修改后的数据对象，包括在嵌套数据集合中与`children`匹配的项
    return modifiedData;
  }

  // 如果检索词不需要显示则返回 `null`
  return null;
}

$(".js-example-matcher-start").select2({
  matcher: matchStart
});

</script>

>>> 支持v3-style匹配回调函数[兼容模块](/upgrading/migrating-from-35#wrapper-for-old-style-matcher-callbacks)。

##  检索词最小长度

有当处理大的数据集时，只有当搜索条目是某个确定长度时才开始筛选会更加高效。这在处理远程数据集时很常见，因为它只允许使用有意义的搜索条目。

您可以使用`minimumInputLength`选项来设置最小搜索长度：

```
$('select').select2({
  minimumInputLength: 3 // 当用户输入超过3个字符后才开始检索
});
```

## 搜索词最大长度

有时候，需要限定一个特定的检索范围。Select2允许您限制搜索词的长度，比如不得超过某个长度。

您可以用`maximumInputLength`选项来设置搜索次的最大长度。

```
$('select').select2({
    maximumInputLength: 20 // 仅允许检索词最多为20个字符
});
```

## 将搜索框的显示限定在大数据集。

`minimumResultsForSearch`选项决定了显示搜索框的下拉菜单初始填充所需要的最小结果数。
 
这个选项对于本地数据和小结果集同时使用的场景很有用。此时搜索框对屏幕实际使用面积纯属浪费。将这个选项的值设为-1，可永久隐藏该搜索框。

```
$('select').select2({
    minimumResultsForSearch: 20 // 至少显示20个结果
});
```

## 搜索框隐藏

### 单选框

对于单选框，Select2允许您使用`minimumResultsForSearch`配置来隐藏搜索框。下面这个例子中，我们用`Infinity`的值来告诉Select2永远不要显示搜索框。

<div class="s2-example">
    <select id="js-example-basic-hide-search" class="js-states form-control"></select>
</div>

<pre data-fill-from="#js-code-example-basic-hide-search"></pre>

<script type="text/javascript" id="js-code-example-basic-hide-search">

$("#js-example-basic-hide-search").select2({
    minimumResultsForSearch: Infinity
});

</script>

### Multi-select 多选

对于多选框，没有独特的搜索控制。所以，要为多选框禁用搜索，无论下拉菜单是打开的还是关闭的，都需要设置`disabled`属性为真值：

<div class="s2-example">
    <select id="js-example-basic-hide-search-multi" class="js-states form-control" multiple="multiple"></select>
</div>

<pre data-fill-from="#js-code-example-basic-hide-search-multi"></pre>

<script type="text/javascript" id="js-code-example-basic-hide-search-multi">

$('#js-example-basic-hide-search-multi').select2();

$('#js-example-basic-hide-search-multi').on('select2:opening select2:closing', function( event ) {
    var $searchfield = $(this).parent().find('.select2-search__field');
    $searchfield.prop('disabled', true);
});
</script>

参考[这个问题](https://github.com/select2/select2/issues/4797)来找到这个方案的源。
