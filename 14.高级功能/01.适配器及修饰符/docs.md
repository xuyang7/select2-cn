---
title: 适配器和修饰符
taxonomy:
    category: docs
---

从4.0 版本开始，Select2 引入[适配器模式](https://en.wikipedia.org/wiki/Adapter_pattern)扩展特性和行为。

之前章节所介绍的内置特性，大部分是通过[内置适配器](/advanced/default-adapters)植入的。但也通过实现自己的适配器，进一步扩展Select2的功能。

## 适配器接口

所有自定义的适配器都必须实现`Adapter`接口所描述的方法。

除此之外，重写 `selectionAdapter` 和 `dataAdapter`适配器，还必须实现相应的`selectionAdapter`和`dataAdapter`接口所描述的附加方法。

### `适配器`

所有适配器都必须实现`Adapter`接口，Select2 通过该接口渲染DOM元素和绑定所有的内部事件：

```
// 由 Select2 渲染的基本 HTML。必须返回一个jQuery 或者 DOM 元素，该jQuery 或者 DOM 元素将自动被Select2 放置在DOM中。
//
// @returns jQuery 或者 DOM 元素，该元素包含所有需要经Select2 渲染的元素。 
Adapter.render = function () {
  return $jq;
};

// 绑定至任意的Select2 或者 DOM 事件
//
// @param container 绑定到jQuery元素上的Select2对象。
// 您可以使用`on` 来监听Select2事件
// 和使用`trigger`方法触发Select2事件
// @param $container jQuery DOM 节点,所有默认的适配器都在当中渲染
Adapter.bind = function (container, $container) { };

// 将DOM元素放置在Select2 DOM容器内，或者放置在其他地方。这允许适配器安放在Select2 DOM 之外，
// 比如在文档的末尾或者Select2 DOM 节点指定的位置。
//
// 注意: 数据适配器中不会调用该方法。
//
// @param $rendered 调用`render`返回的已经被渲染过的DOM元素。
// 该DOM元素很可能已经被Select2组件修改过，但始终保证root元素不变。
// @param $defaultContainer Select2 放置 DOM 元素的默认容器。
// 对大多数适配器而言，这就是Select2 DOM元素。
Adapter.position = function ($rendered, $defaultContainer) { };

// 所有创建的事件和DOM元素都必须释放。
// 当某个元素调用 `select2("destroy")`时候，将会调用此方法。
Adapter.destroy = function () { };
```

### `SelectionAdapter`

Selection是展示给用户的，以替换标准的`<select>`框。它控制了选中选项的展示方式，和其他需要嵌入容器的其他内容，比如搜索框。


如果需要重写默认`selectionAdapter`适配器的，那么自定义的适配器同样得实现`update`方法：


```
// 更新选中数据。
//
// @param data 数据对象数组，通过data 适配器生成。
// 如果没有选中对象，则返回空数组。
//
// 注意: 该方法总是以数组形式传递，在Select2单选的情况下，也是如此。
SelectionAdapter.update = function (data) { };
```

### `DataAdapter` 


数据集是Select2用来生成可以选择的可能结果以及当前所选结果的方法。

 
将用于重写默认`dataAdapter` 的自定义适配器也必须实现`current` 和`query`方法。


```
// 获取当前选中的选项。当试图获取Select2初始选项时，或者Select2需要确定那些结果被选中，就会调用此方法。
//
// @param callback 回调函数，当前选项已经完成检索时调用。该函数的第一个参数必须是数据对象数组。
DataAdapter.current = function (callback) {
  callback(currentData);
}

// 获取一组基于传入的参数进行筛选的选项。
//
// @param params 包含用于查询的任意数量的参数。仅记录核心参数。
// @param params.term 用户提供的短语。通常是搜索框的值，如果存在，但也可以使空字符串或null。
// @param params.page 必须加载的指定页面数据。
// 通常用于处理远程数据集，依赖分页来保证该展示哪些数据。
// @param callback 函数，查询结果完成后调用。
DataAdapter.query = function (params, callback) {
  callback(queryiedData);
}
```

## 修饰符

Select2通过其[配置选项](/configuration)使用[修饰符](https://en.wikipedia.org/wiki/Decorator_pattern)来导出适配器的功能。

您可以用Select2提供的`Utils.Decorate`方法，将修饰符应用到适配器上：

```
$.fn.select2.amd.require(
    ["select2/utils", "select2/selection/single", "select2/selection/placeholder"],
    function (Utils, SingleSelection, Placeholder) {
  var CustomSelectionAdapter = Utils.Decorate(SingleSelection, Placeholder);
});
```

>>> 所有使用修饰符或适配器的核心选项都会在文件的“修饰符”或“适配器”中明确地陈述。修饰符只与特定的适配器类型相兼容，所以请确保注意用的是什么适配器。

## AMD 兼容

现有基于AMD构建的项目如何集成Select2，[这里](/getting-started/builds-and-modules)可以找到更多信息。 当适配器被自动构建后，Select2 会自动加载一些模块，因此使用AMD方式构建的Select2用户，可能需要指定Select2模块的安装目录。