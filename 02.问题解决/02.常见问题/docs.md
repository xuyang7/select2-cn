---
标题: 常见问题
元数据:
    描述: Select2 使用过程中的常见问题
分类:
    目录: docs
---

### Bootstrap 模态框（modal）中 Select2 无法正常使用

这个问题是由于Bootstrap 模态框（modal）通过从 modal 之外其他元素获取焦点。默认情况下，Select2 [下拉框是同`<body>`元素关联](/dropdown#dropdown-placement)的，所以被误认为“模态框之外”。

可通过[dropdownParent](/dropdown#dropdown-placement)配置将下拉框与modal绑定解决该问题：

```
<div id="myModal" class="modal fade" tabindex="-1" role="dialog" aria-hidden="true">
    ...
    <select id="mySelect2">
        ...
    </select>
    ...
</div>

...

<script>
    $('#mySelect2').select2({
        dropdownParent: $('#myModal')
    });
</script>
```

结果是将下拉框绑定在 Modal 上，而不是`<body>` 标签上。

**或者**, 更为简单的，重写 Bootstrap：

```
//初始化任何 modal 对象时之前，这样做
$.fn.modal.Constructor.prototype.enforceFocus = function() {};
```

更多信息请参考[此回答](https://stackoverflow.com/questions/18487056/select2-doesnt-work-when-embedded-in-a-bootstrap-modal/19574076#19574076)。

### 缩放时下拉框没对齐/移位

详见[#5048](https://github.com/select2/select2/issues/5048).  原因是：缩放时某些浏览器会影响整个`body`，而[Select2 默认绑在body](https://select2.org/dropdown#dropdown-placement)，从而导致渲染不正确。

解决方案：使用`dropdownParent`参数将下拉框与指定的元素绑定。