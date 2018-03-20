---
标题: 外观
分类:
    目录 docs
process:
    twig: true
never_cache_twig: true
---

Select2 控件外观有 2 种方式自定义方式，通过标准的 HTML `<select>`标签，和[配置项](/configuration)。 

## 禁用 Select2 控件

在`<select>` 标签上使用<code>disabled</code>将禁用Select2 控件，或者使用
`disabled: true` 初始化 Select2 也能达到相同的效果。

<div class="s2-example">
  <p>
    <select class="js-example-disabled js-states form-control" disabled="disabled"></select>
  </p>

  <p>
    <select class="js-example-disabled-multi js-states form-control" multiple="multiple" disabled="disabled"></select>
  </p>
  <div class="btn-group btn-group-sm" role="group" aria-label="Programmatic enabling and disabling">
    <button type="button" class="js-programmatic-enable btn btn-default">
      Enable
    </button>
    <button type="button" class="js-programmatic-disable btn btn-default">
      Disable
    </button>
  </div>
</div>

<pre data-fill-from=".js-code-disabled"></pre>

<script type="text/javascript" class="js-code-disabled">

$(".js-example-disabled").select2();
$(".js-example-disabled-multi").select2();
  
$(".js-programmatic-enable").on("click", function () {
  $(".js-example-disabled").prop("disabled", false);
  $(".js-example-disabled-multi").prop("disabled", false);
});

$(".js-programmatic-disable").on("click", function () {
  $(".js-example-disabled").prop("disabled", true);
  $(".js-example-disabled-multi").prop("disabled", true);
});

</script>

## 标签

Select2 跟其他`<select>` 元素一样，使用前需添加 `<label>` 标签。

<div class="s2-example">
  <p>
    <label for="id_label_single">
      单击此可查看单选框
      <select class="js-example-basic-single js-states form-control" id="id_label_single"></select>
    </label>
  </p>
  <p>
    <label for="id_label_multiple">
      单击此可查看多选框
      <select class="js-example-basic-multiple js-states form-control" id="id_label_multiple" multiple="multiple"></select>
    </label>
  </p>
</div>

```
<label for="id_label_single">
  单击此实现单选框高亮显示

  <select class="js-example-basic-single js-states form-control" id="id_label_single"></select>
</label>

<label for="id_label_multiple">
 单击此实现多选框高亮展示

  <select class="js-example-basic-multiple js-states form-control" id="id_label_multiple" multiple="multiple"></select>
</label>
```

<script type="text/javascript">
  $.fn.select2.amd.require([
    "select2/core",
    "select2/utils"
  ], function (Select2, Utils, oldMatcher) {
    var $basicSingle = $(".js-example-basic-single");
    var $basicMultiple = $(".js-example-basic-multiple");

    $.fn.select2.defaults.set("width", "100%");

    $basicSingle.select2();
    $basicMultiple.select2();

    function formatState (state) {
      if (!state.id) {
        return state.text;
      }
      var $state = $(
        '<span>' +
          '<img src="vendor/images/flags/' +
            state.element.value.toLowerCase() +
          '.png" class="img-flag" /> ' +
          state.text +
        '</span>'
      );
      return $state;
    };
  });

</script>

## 容器宽度

Select2 会尽量匹配原始元素的宽度，但有时效果不佳，此时可通过配置选项`width` [配置选项](/configuration)手动设置：

<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <th>值</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>'element'</code></td>
      <td>
        从任何可用的CSS规则中计算元素宽度。
      </td>
    </tr>
    <tr>
      <td><code>'style'</code></td>
      <td>
        Width 由 <code>select</code> 元素 <code>style</code> 属性决定，如果找不到 <code>style</code> 属性, width 则为 null。
      </td>
    </tr>
    <tr>
      <td><code>'resolve'</code></td>
      <td>
        如果 <code>style</code> 属性值可用，则直接使用，必要时再使用必要时返回计算的元素的宽度。
      </td>
    </tr>
    <tr>
      <td><code>'&lt;value&gt;'</code></td>
      <td>
        字符串也可以做为有效的 CSS 值(例如 <code>width: '80%'</code>).
      </td>
    </tr>
  </tbody>
</table>

### 示例

下面 2 个 Select2 框的宽度分别被置为`50%` 和`75%`以实现交互：
The two Select2 boxes below are styled to `50%` and `75%` width respectively to support responsive design:

<div class="s2-example">
  <p>
    <select class="js-example-responsive js-states" style="width: 50%"></select>
  </p>
  <p>
    <select class="js-example-responsive js-states" multiple="multiple" style="width: 75%"></select>
  </p>
</div>

```
<select class="js-example-responsive" style="width: 50%"></select>
<select class="js-example-responsive" multiple="multiple" style="width: 75%"></select>
```

<pre data-fill-from=".js-code-example-responsive"></pre>

<script type="text/javascript" class="js-code-example-responsive">

$(".js-example-responsive").select2({
    width: 'resolve' // 需覆盖修改后的默认值
});

</script>

>>>> Select2 会尽力解决通过CSS 类指定的百分比宽度，但有时也会出错。确保 Select2 通过百分比能计算出宽度的最佳方案是在标签中声明`style` 属性。

## 主题

使用 `theme`  选项可自定义 Select2 主题，从而与应用的其他主题保持一致。

这些例子使用`classic` 主题，与 Select2 旧主题相同。

<div class="s2-example">
  <p>
    <select class="js-example-theme-single js-states form-control">
    </select>
  </p>
  <p>
    <select class="js-example-theme-multiple js-states form-control" multiple="multiple"></select>
  </p>
</div>

<pre data-fill-from=".js-code-example-theme"></pre>

<script type="text/javascript" class="js-code-example-theme">

$(".js-example-theme-single").select2({
  theme: "classic"
});

$(".js-example-theme-multiple").select2({
  theme: "classic"
});

</script>

Select2 组件的各种展示项都可以修改，使用`.element`可访问`<option>` （或 `<optgroup>`）任何属性。