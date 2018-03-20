---
title: 增加，选择或清除菜单项
metadata: 
    description:在Select2中以编程方式增加、选择并清除选项
taxonomy:
    category: docs 
---

## 在下拉菜单中创建新选项

新选项可以通过编程方式创建新的[Javascript `Option` 对象](https://developer.mozilla.org/en-US/docs/Web/API/HTMLOptionElement/Option)附加到Select2控件上：

```
var data = {
    id: 1,
    text: 'Barn owl'
};

var newOption = new Option(data.text, data.id, false, false);
$('#mySelect2').append(newOption).trigger('change');
```

`new Option(...)`的第三个参数决定了下拉项是否为“默认选择”；比如，它为新下拉项设置了`selected`属性。第4个参数设置了下拉项为实际选择状态--如果设置为`true`，默认会选择新选项。

### 如果不存在就创建

You can use `.find` to select the option if it already exists, and create it otherwise: 如果下拉项已经存在，您可以使用`.find`来选择下拉项，如果不存在，则进行创建。

```
// 设置新值，必要时创建新选项
if ($('#mySelect2').find("option[value='" + data.id + "']").length) {
    $('#mySelect2').val(data.id).trigger('change');
} else { 
    // 新建DOM选项并默认选中
    var newOption = new Option(data.text, data.id, true, true);
    // 附加到select控件上
    $('#mySelect2').append(newOption).trigger('change');
} 
```

## 选中选项

要以编程方式为Select2选中一个选项/菜单,可使用jQuery `.val()` 方法：

```
$('#mySelect2').val('1'); // 选中值为1的选项
$('#mySelect2').trigger('change'); // 通知所有JS组件，选项值已经发生变化
```

您也可以传递数组到`val`实现多选：

```
$('#mySelect2').val(['1', '2']);
$('#mySelect2').trigger('change'); //通知所有JS组件，选项值已经发生变化 
```

Select2会监听附在`<select>`元素上的 `change`事件。当您做任何外部变更需要反映在Select2中时（比如改变值时），都会触发本事件。

### 在远程源（AJAX）中预选中选项

对于从[AJAX 源](/data-sources/ajax)接收数据的Select2控件用`.val()`无法设置选项选中。当前选项还不不存在，因为AJAX请求直到控件打开或用户开始搜索时才会被激活。这使得通过服务器端的过滤和标识页码变得更加复杂，不能保证某个特殊选项确实加载到Select控件中。

因此，最好的处理办法是简单地增加预选条目作为一个新选项。对于远程源数据，这将很可能包括在您的服务器端的应用中创建一个新的API节点，可以检索单个条目。

```
// Set up the Select2 control
$('#mySelect2').select2({
    ajax: {
        url: '/api/students'
    }
});

// 获取预选项，并添加到Select2中
var studentSelect = $('#mySelect2');
$.ajax({
    type: 'GET',
    url: '/api/students/s/' + studentId
}).then(function (data) {
    // 创建选项并附加到Select2控件
    var option = new Option(data.full_name, data.id, true, true);
    studentSelect.append(option).trigger('change');

    // 手动触发 `select2:select` 事件
    studentSelect.trigger({
        type: 'select2:select',
        params: {
            data: data
        }
    });
});
```

注意我们手动触发`select2:select`事件并传递整个`data`对象。这允许其他的操作者[访问已选条目的附加属性](/programmatic-control/events#triggering-events).

## 清除选项

您可以通过将Select2控件的值设置`null`来清除当前选中项。

```
$('#mySelect2').val(null).trigger('change');
```
