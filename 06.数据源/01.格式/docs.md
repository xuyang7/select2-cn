---
标题：Select2数据格式
分类：
    目录：docs
---

Select2可以通过编程方式将一个数组或远程数据源（AJAX）提供的数据作为下拉选项。为此，Select2需要非常具体的数据格式。这个格式包含JSON对象，JSON对象包含由`results`键键入的对象数组。

```
{
  "results": [
    {
      "id": 1,
      "text": "Option 1"
    },
    {
      "id": 2,
      "text": "Option 2"
    }
  ],
  "pagination": {
    "more": true
  }
}
```

每个对象应_至少包含_一个`id`和一个`text`属性。通过数据对象传递的任何其他参数都将包含在Select2公开的数据对象上。

如果您想要使用无限滚动特性，响应对象也可以包含标记页码数据。通过`pagination`关键字指定。

## 选择和禁用选项

您也可以在这个数据结构中为该选项提供`selected`和`disabled`属性。比如：
 
```
{
  "results": [
    {
      "id": 1,
      "text": "Option 1"
    },
    {
      "id": 2,
      "text": "Option 2",
      "selected": true
    },
    {
      "id": 3,
      "text": "Option 3",
      "disabled": true
    }
  ]
}
```

在这个例子里，选项2将会被预选，而选项3将会被[禁用](/options#disabling-options)。

## 将数据转换成需要的格式

### 生成`id` 属性

Select2需要`id`属性用于唯一定义在结果清单中显示的选项。如果您使用`id`以外的属性（比如`pk`）来唯一定义一个选项，您需要在将其传递到Select2之前，映射旧属性到`id`。

如果在您的服务器上无法实现，或者API无法修改的情况下下，在传入Select2之前，您可以在Javascript中这么做：

```
var data = $.map(yourArrayData, function (obj) {
  obj.id = obj.id || obj.pk; // 将id替换pk

  return obj;
});
```

### 生成`text`属性

就像使用`id`属性一样，Select2要求应该显示为选项的文本存储在`text`属性中。您可以使用以下JavaScript从任何现有属性映射该属性:

```
var data = $.map(yourArrayData, function (obj) {
  obj.text = obj.text || obj.name; // 将text替换为name

  return obj;
});
```

## 自动字符串计算

因为`<select>`标签上的`value`属性必须是字符串，并且Select2从数据对象的`id`属性中生成`value`属性， 每个数据对象的`id`属性也必须是一个字符串。

Select2试图将任何非字符串转换为字符串，这在大多数情况下都有效但是推荐尽早将您的`id`转换为字符串。

不允许有空的`id`或者值为0的`id`。

## 分组数据

当在`<optgroup>`分区中生成选项时，选项应该嵌套在每个组对象的`children` 关键字下。组的标签应指定为组对应数据对象上的`text`属性。
```
{
  "results": [
    { 
      "text": "Group 1", 
      "children" : [
        {
            "id": 1,
            "text": "Option 1.1"
        },
        {
            "id": 2,
            "text": "Option 1.2"
        }
      ]
    },
    { 
      "text": "Group 2", 
      "children" : [
        {
            "id": 3,
            "text": "Option 2.1"
        },
        {
            "id": 4,
            "text": "Option 2.2"
        }
      ]
    }
  ],
  "paginate": {
    "more": true
  }
}
```

>>>> Because Select2 generates an `<optgroup>` when creating nested options, only [a single level of nesting is supported](/options#dropdown-option-groups). Any additional levels of nesting is not guaranteed to be displayed properly across all browsers and devices. 因为Selecr2在创建嵌套选项时生成一个`<optgroup>`，[只支持单层嵌套](/options#dropdown-option-groups)。无法保证任何其它额外层次在所有浏览器和设备得能显示。
