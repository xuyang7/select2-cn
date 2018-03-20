---
标题：动态选项创建
分类：
    目录：docs
process:
    twig: true
never_cache_twig: true
---

除了预填充的选项菜单以外，Select2可以从搜索框内用户输入的文本中动态创建新选项。这项功能叫做“标记”。要激活标记功能，可设置`tags`选项为`true`:

<div class="s2-example">
  <p>
    <select class="js-example-tags form-control">
      <option selected="selected">orange</option>
      <option>white</option>
      <option>purple</option>
    </select>
  </p>
</div>

```
<select class="form-control">
  <option selected="selected">orange</option>
  <option>white</option>
  <option>purple</option>
</select>

$(".js-example-tags").select2({
  tags: true
});
```

注意当标记功能激活时，用户可以从预存的选项中选择或者通过第一个选项来创建新选项，也就是用户当前输入到搜索框中的内容。

## 多值选择框标记

在多值选择框内也可以使用标记。下面的例子中，通过设置Select2`multiple="multiple"`属性，也可以激活 `tags: true`：
  
<div class="s2-example">
  <p>
    <select class="js-example-tags form-control" multiple="multiple">
      <option selected="selected">orange</option>
      <option>white</option>
      <option selected="selected">purple</option>
    </select>
  </p>
</div>

```
<select class="form-control" multiple="multiple">
  <option selected="selected">orange</option>
  <option>white</option>
  <option selected="selected">purple</option>
</select>
```

<script type="text/javascript">

$(".js-example-tags").select2({
  tags: true
});

</script>

输入一个下拉菜单中没有的值，您可以将其增加为一个新的选项！

## 标记内自动符号解析

当用户输入检索字段时，Select2支持自动添加选择功能。试着输入以下检索字段并输入一个空格或者逗号。

使用`tokenSeparators`选项可以指定符号解析时，可以使用该分隔符。

<div class="s2-example">
<p>
  <select class="js-example-tokenizer form-control" multiple="multiple">
    <option>red</option>
    <option>blue</option>
    <option>green</option>
  </select>
</p>
</div>

<pre data-fill-from=".js-code-example-tokenizer"></pre>

<script type="text/javascript" class="js-code-example-tokenizer">

$(".js-example-tokenizer").select2({
    tags: true,
    tokenSeparators: [',', ' ']
})

</script>

## 自定义tag创建

### Tag 属性

您可以通过定义`createTag`回调来给新创建的标记增加额外属性：

```
$('select').select2({
  createTag: function (params) {
    var term = $.trim(params.term);

    if (term === '') {
      return null;
    }

    return {
      id: term,
      text: term,
      newTag: true // add additional parameters
    }
  }
});
```

### 受限tag创建

Select2允许用户创建新标记，当用户输入一个无效值时，您也可以通过给`createTag`添加函数逻辑处理`null`：

```
$('select').select2({
  createTag: function (params) {
    // 如果包含 @ 符号，则不允许新建 tag 
    if (params.term.indexOf('@') === -1) {
      // 返回 null，禁用 tag 创建
      return null;
    }

    return {
      id: params.term,
      text: params.term
    }
  }
});
```

## 下拉菜单中自定义标记位置

您可以通过定义`insertTag` 回调来控制新创建选项的位置。

```
$('select').select2({
  insertTag: function (data, tag) {
    // 将tag 加入到结果集后
    data.push(tag);
  }
});
```
