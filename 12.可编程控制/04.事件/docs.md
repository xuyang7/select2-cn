---
title: 事件
metadata:
    description: 监听Select2'内置事件，并手动触发Select2组件上的事件。
taxonomy: 
    category: docs
process:
    twig: true
never_cache_twig: true
---
 
当组件操作不同的时，Select2会触发一些不同的事件，允许您增加定制的钩子和执行操作。您也可以用`.trigger`在Select2上手动触发这些事件。

| 事件| 描述|
| ----- | ----------- |
| `change` | 选择或移除一个选项时而触发|
| `change.select2` | `change`的作用域的版本。详见|[如下](#limiting-the-scope-of-the-change-event)
| `select2:closing` |在下拉菜单关闭前触发，可阻止。|
| `select2:close` | 只要下拉菜单关闭就触发，可阻止。`select2:closing` 事件早于该事件触发|
| `select2:opening` | 在下拉菜单打开前触发，可阻止 | 在下拉菜单打开前触发。这个事件可以预防
| `select2:open` | 下拉菜单打开时触发，可阻止。`select2:opening` 早于该事件触发。
| `select2:selecting` | 在结果选择之前触发，可阻止。
| `select2:select` | 选择结束后立即触发，可阻止。`select2:selecting`  事件早于该事件触发。
| `select2:unselecting` | 选择移除之前触发，可阻止。
| `select2:unselect` | 一旦有选择被移除就触发，可阻止。 `select2:unselecting` 事件早于该事件。

## 监听事件

所有公共事件都通过jQuery事件系统互相传递，
并通过Select2绑定的`<select>`元素触发。您可以使用jQuery提供的[`.on` 方法](https://api.jquery.com/on/)绑定：

```
$('#mySelect2').on('select2:select', function (e) {
  // 干点什么
});
```

## 事件数据

当`select2:select`被触发时，可通过`params.data` 属性访问已选项：

```
$('#mySelect2').on('select2:select', function (e) {
    var data = e.params.data;
    console.log(data);
});
```

`e.params.data`会返回一个包含选择属性的对象。任何[数据源](/data-sources/formats)提供的附加数据也会被加载到这个对象里，如：

```
{
  "id": 1,
  "text": "Tyto alba",
  "genus": "Tyto",
  "species": "alba"
}
```

## 触发事件

您可以使用jQuery[`trigger`](http://api.jquery.com/trigger/)方法在Select2控件上手动触发事件。但是，如果您想要传递一些数据到处理器中，您需要在事件上构建一个新的[jQuery `事件` 对象](http://api.jquery.com/category/events/event-object/)，然后触发：

```
var data = {
  "id": 1,
  "text": "Tyto alba",
  "genus": "Tyto",
  "species": "alba"
};

$('#mySelect2').trigger({
    type: 'select2:select',
    params: {
        data: data
    }
});
```

###  限制`change`事件的范围

对于其它组件来说，监听`change`事件是很常见的，或者说对要附加的定制事件处理器有副作用。为了限制其范围为**只**通知Select2变更的部分，可使用`.select2`事件的命名空间：

```
$('#mySelect2').val('US'); // 修改值，或者修改内部状态
$('#mySelect2').trigger('change.select2'); // 仅在Select2修改时触发
```

## 举例

<div class="s2-example">
  <p>
    <select class="js-states js-example-events form-control"></select>
  </p>
  <p>
    <select class="js-states js-example-events form-control" multiple="multiple"></select>
  </p>
</div>

<div class="s2-event-log">
  <ul class="js-event-log"></ul>
</div>

<pre data-fill-from=".js-code-events"></pre>

<script type="text/javascript" class="js-code-events">
var $eventLog = $(".js-event-log");
var $eventSelect = $(".js-example-events");

$eventSelect.select2();

$eventSelect.on("select2:open", function (e) { log("select2:open", e); });
$eventSelect.on("select2:close", function (e) { log("select2:close", e); });
$eventSelect.on("select2:select", function (e) { log("select2:select", e); });
$eventSelect.on("select2:unselect", function (e) { log("select2:unselect", e); });

$eventSelect.on("change", function (e) { log("change"); });

function log (name, evt) {
  if (!evt) {
    var args = "{}";
  } else {
    var args = JSON.stringify(evt.params, function (key, value) {
      if (value && value.nodeName) return "[DOM node]";
      if (value instanceof $.Event) return "[$.Event]";
      return value;
    });
  }
  var $e = $("<li>" + name + " -> " + args + "</li>");
  $eventLog.append($e);
  $e.animate({ opacity: 1 }, 10000, 'linear', function () {
    $e.animate({ opacity: 0 }, 2000, 'linear', function () {
      $e.remove();
    });
  });
}
</script>

## 阻碍事件

参见[https://stackoverflow.com/a/26706695/2970321](https://stackoverflow.com/a/26706695/2970321)。

## Select2内部事件

Select2有一个[内部事件系统](/advanced/default-adapters/selection#eventrelay)，它独立于DOM事件系统而工作，允许适配器彼此沟通。该内部事件系统仅可从连接到Select2的插件和适配器访问，而**不**是通过jQuery事件系统。

在[advanced 章节](/advanced)里的独立适配器触发的公共事件，可供您找到更多信息。
