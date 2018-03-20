---
标题: 方法
元数据:
    描述: Select2 提供了部分内置的方法，拥有对组件进行可编程控制。 
分类:
    目录: docs
process:
    twig: true
never_cache_twig: true
---

Select2 提供了部分内置的方法，拥有对组件进行可编程控制。 

## 打开下拉框

通过将方法名传递给`.select2(...)`，可以直接调用Select2处理的方法。

`open`方法会打开下拉菜单打开，显示可选择的选项：

```
$('#mySelect2').select2('open');
```

## 关闭下拉框

`close`方法会关闭下拉菜单关闭，隐藏可选择的选项：

```
$('#mySelect2').select2('close');
```

## 检查插件是否已完成初始化

为了测试Select2是否已在特定的DOM元素上初始化，您可以验证`select2-hidden-accessible`类：

```
if ($('#mySelect2').hasClass("select2-hidden-accessible")) {
    // Select2 已完成初始化
}
```

参考[这个Stack Overflow回答](https://stackoverflow.com/a/29854133/2970321)

##  销毁Select2控件

`destroy`方法会从目标元素中移除Select2。，然后恢复原来标准`select`控件。
  
```
$('#mySelect2').select2('destroy');
```

### 事件解绑定

Select2 控件销毁时，Select2 只会解绑定那些通过插件自动绑定的事件，对于其他任何用户代码定义事件，**包括任何显式绑定的[Select2 事件](/programmatic-control/events)**都必须使用`.off`  jQuery 方法手动解绑:

```
// 创建Select2控制器
$('#example').select2();

// 绑定事件
$('#example').on('select2:select', function (e) { 
    console.log('select event');
});

// 销毁Select2
$('#example').select2('destroy');

// 事件解绑定
$('#example').off('select2:select');
```

## 示例

<div class="s2-example">

    <label for="select2-single">Single select</label>
    
    <button class="js-programmatic-set-val button" aria-label="Set Select2 option">
      Set "California"
    </button>
    
    <button class="js-programmatic-open button">
      Open
    </button>
    
    <button class="js-programmatic-close button">
      Close
    </button>
    
    <button class="js-programmatic-destroy button">
      Destroy
    </button>
    
    <button class="js-programmatic-init button">
      Re-initialize
    </button>
    <p>
      <select id="select2-single" class="js-example-programmatic js-states form-control"></select>
    </p>
    
    <label for="select2-multi">Multiple select</label>

    <button type="button" class="js-programmatic-multi-set-val button" aria-label="Programmatically set Select2 options">
      Set to California and Alabama
    </button>
    
    <button type="button" class="js-programmatic-multi-clear button" aria-label="Programmatically clear Select2 options">
      Clear
    </button>

    <p>
      <select id="select2-multi" class="js-example-programmatic-multi js-states form-control" multiple="multiple"></select>
    </p>

</div>

<pre data-fill-from=".js-code-programmatic"></pre>

<script type="text/javascript" class="js-code-programmatic">

var $example = $(".js-example-programmatic").select2();
var $exampleMulti = $(".js-example-programmatic-multi").select2();

$(".js-programmatic-set-val").on("click", function () {
    $example.val("CA").trigger("change");
});

$(".js-programmatic-open").on("click", function () {
    $example.select2("open");
});

$(".js-programmatic-close").on("click", function () {
    $example.select2("close");
});

$(".js-programmatic-init").on("click", function () {
    $example.select2();
});

$(".js-programmatic-destroy").on("click", function () {
    $example.select2("destroy");
});

$(".js-programmatic-multi-set-val").on("click", function () {
    $exampleMulti.val(["CA", "AL"]).trigger("change");
});

$(".js-programmatic-multi-clear").on("click", function () {
    $exampleMulti.val(null).trigger("change");
});

</script>
