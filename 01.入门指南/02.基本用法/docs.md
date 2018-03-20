---
标题：基本用法
分类：
    目录：docs
process:
    twig: true
never_cache_twig: true
---

## 单选框

Select2是为替代浏览器显示的标准`<select>`框而设计的。它默认支持所有单选框里的选项和操作，并且更加灵活。
 
Select2可以处理像这样的常规选择框...

<select class="js-states form-control"></select>

并将其转换成这个...

<div class="s2-example">
    <select class="js-example-basic-single js-states form-control"></select>
</div>

```
<select class="js-example-basic-single" name="state">
  <option value="AL">Alabama</option>
    ...
  <option value="WY">Wyoming</option>
</select>
```

<script type="text/javascript" class="js-code-example-basic-single">
$(document).ready(function() {
    $('.js-example-basic-single').select2();
});
</script>

如果您使用任何一种分布式构建，Select2会将自身以jQuery函数方式注册，所以当您想要初始化Select2时，您可以在任何jquery选择符上调用`.select2()`。

```
// 在你的Javascript (外部 .js 资源或者 <script> 标签)
$(document).ready(function() {
    $('.js-example-basic-single').select2();
});
```

>>>>>> DOM只有在加载完成后才能操作。为了保证浏览器在初始化Select2控制之前，准备好您的DOM，将您的代码包装进[`$(document).ready()`](https://learn.jquery.com/using-jquery-core/document-ready/) 模块。每个页面只需要一个`$(document).ready()`模块。

## 多选框（pillbox）

Select2也支持多选择框。下面的选择以`multiple`属性进行声明。

<div class="s2-example">
  <p>
    <select class="js-example-basic-multiple js-states form-control" multiple="multiple"></select>
  </p>
</div>

**在你的HTML代码里:**

```
<select class="js-example-basic-multiple" name="states[]" multiple="multiple">
  <option value="AL">Alabama</option>
    ...
  <option value="WY">Wyoming</option>
</select>
```

**在你的Javascript文件 (外部 `.js` 资源或者`<script>` 标签):**

```
$(document).ready(function() {
    $('.js-example-basic-multiple').select2();
});
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
