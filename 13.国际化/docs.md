---
标题: 国际化
分类:
    目录: docs
process:
    twig: true
never_cache_twig: true
---

{% do assets.addJs('https://cdnjs.cloudflare.com/ajax/libs/select2/4.0.3/js/i18n/es.js', 90) %}

## 消息翻译

必要时, Select2 为用户展示特定消息，比如，当没有检索结果或者需要输入更多字符才能获得检索结果。这些消息已经被Select2贡献者翻译成多种语言，但你仍可提供自己翻译的版本。

### 语言文件

Select2可以为不同的语言加载不同的语言文件。 当使用Select2 提供的翻译功能时候，必须确保在Select2后包含你的翻译文件。

当语言以字符串形式参数传入时，Select2 将尝试将其转换为语言文件。这允许你指定自己的语言文件，但必须通过AMD 模块的形式定义。如果找不到语言文件，Select2 会认为是自己内置的语言，因此会试着加载该语言文件。

<div class="s2-example">
    <p>
      <select class="js-example-language js-states form-control">
      </select>
    </p>
</div>

```
$(".js-example-language").select2({
  language: "es"
});
```

<script type="text/javascript">
    $(".js-example-language").select2({
      language: "es"
    });
</script>

初始化Select2时，不需要定义语言，但可以在任意父元素的`[lang]` 属性上定义为`[lang="es"]`。

### 翻译对象

也可以通过下面类似的对象，自定义消息：

```
language: {
  // 您可以在你的构建后的语言文件中找到所有选项。
  // 它们必须是函数，并且返回将要显示的字符串。
  inputTooShort: function () {
    return "You must enter more characters...";
  }
}
```

>>> 翻译功能经由`select2/translation` 模块处理。

## RTL 支持

如果在`<select>`上或者它的父元素上设置`dir` 属性，RTL 网页上也可使用Select2。您还可以使用 `dir: "rtl"` 配置项初始化Select2。

<div class="s2-example">
    <p>
      <select class="js-example-rtl js-states form-control" dir="rtl"></select>
    </p>
</div>

```
$(".js-example-rtl").select2({
  dir: "rtl"
});
```

<script type="text/javascript">
    $(".js-example-rtl").select2({
      dir: "rtl"
    });
</script>

## 音译支持 (变音符号)


Select2 默认匹配器会将修改后的字符转换成ASCII码，方便用户在国际化选择中过滤结果。输入"aero" 进入下面的选择：

<div class="s2-example">
  <p>
    <select class="js-example-diacritics form-control">
      <option>Aeróbics</option>
      <option>Aeróbics en Agua</option>
      <option>Aerografía</option>
      <option>Aeromodelaje</option>
      <option>Águilas</option>
      <option>Ajedrez</option>
      <option>Ala Delta</option>
      <option>Álbumes de Música</option>
      <option>Alusivos</option>
      <option>Análisis de Escritura a Mano</option>
    </select>
  </p>
</div>

```
$(".js-example-diacritics").select2();
```

<script type="text/javascript">
    $(".js-example-diacritics").select2();
</script>
