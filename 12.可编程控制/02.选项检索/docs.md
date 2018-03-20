---
标题：检索选项
元数据：
    描述: 程序中有2种方式访问当前选项的数据：一种是使用`.select2('data')`，另一种是使用jQuery 选择器。
分类：
    目录: docs
---

程序中有2种方式访问当前选项的数据：一种是使用`.select2('data')`，另一种是使用jQuery 选择器。

## 使用 `data` 方法

调用`select2('data')` 将返回当前选项的JavaScript对象数组。数组中的每个对象包含所有通过`processResults` and `templateResult`回调返回的属性/值。

```
$('#mySelect2').select2('data');
```

## 使用jQuery选择器

通过 `:selected` jQuery选择器同样可以访问所选项：

```
$('#mySelect2').find(':selected');
```

可使用HTML `data-*`属性扩展当前选择的`<option>`元素，来包含来自元数据对象的任意数据：

```
$('#mySelect2').select2({
  // ...
  templateSelection: function (data, container) {
    // 将自定义属性添加到所选项的<option>标签
    $(data.element).attr('data-custom-attribute', data.customValue);
    return data.text;
  }
});

// 检索第一个选项元素的自定义属性值
$('#mySelect2').find(':selected').data('custom-attribute');
```

>>>> 不要依赖`option`元素的selected属性确定当前所选项。通过远程数据源创建`selected`元素时，Select2 并不会添加此属性。 更多详细信息，请 [参见此问题](https://github.com/select2/select2/issues/3366#issuecomment-102566500)。