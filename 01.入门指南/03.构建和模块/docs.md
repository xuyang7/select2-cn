---
标题：构建和模块
分类:
    目录: docs
process:
    twig: true
---

## 不同的Select2 构建

Select2 提供了适用于不同应用环境的多种构建。如果您需要在非标环境下使用Select2，比如正在使用AMD，那么您应该仔细阅读以下内容。

<table class="table table-bordered table-striped">
  <thead>
    <tr>
      <th>构建名称</th>
      <th>何时使用</th>
    </tr>
  </thead>
  <tbody>
    <tr id="builds-standard">
      <td>
        Standard (<code>select2.js</code> / <code>select2.min.js</code>)
      </td>
      <td>
        这是多数人使用Select2时最常用的构建，内含大多数常用特性。
      </td>
    </tr>
    <tr id="builds-full">
      <td>
        Full (<code>select2.full.js</code> / <code>select2.full.min.js</code>)
      </td>
      <td>
        只有您需要Select2的附加特性时才能使用这种构建，比如<a href="{{base_url_absolute}}/upgrading/migrating-from-35">兼容模式</a> 或者推荐模式包括像 <a href="https://github.com/jquery/jquery-mousewheel">jquery.mousewheel</a>
      </td>
    </tr>
  </tbody>
</table>

## 通过AMD或者CommonJS加载Select2

Select2在大部分AMD环境下，或者符合CommonJS的加载模块都可以工作，包括[ReqireJS](http://requirejs.org)和[almond](http://github.com/jrburke/almond).Select2提供了[UMD jQuery 模板]的一个修改版本（https://github.com/umdjs/umd/blob/f208d385768ed30cd0025d5415997075345cd1c0/templates/jqueryPlugin.js）同时支持CommonJS和AMD环境。

### 配置

对于大部分AMD和CommonJS的安装，Select2所使用的数据文件的位置会自动确定并处理，无需您做任何操作。

Select2 在内部使用AMD和r.js构建工具在`dist`文件夹中创建文件。这些文件是用`src`文件夹中的文件来创建的，所以当您想要使用Select2时，您只需将您的模块指向Select2的源代码并加载`jquery.select2`或`select2/core`即可。位于`dist`文件夹中的文件也兼容AMD，所以若您想加载所有默认的Select2模块，也可以指向该文件。

如果您在一个构建环境中使用Select2,而环境中预存的模块名在某个构建过程中发生变更，Select2可能会无法找到可选依赖或语言文件。您可以使用`amdBase`和`amdLanguageBase`选项为这些文件手动设置前缀来使用。

```
$.fn.select2.defaults.set('amdBase', 'select2/');
$.fn.select2.defaults.set('amdLanguageBase', 'select2/i18n/');
```

#### `amdBase`

Specifies the base AMD loader path to be used for select2 dependency resolution. This option typically doesn't need to be changed, but is available for situations where module names may change as a result of certain build environments. 
指定用于select2依赖性解析的基础AMD加载器路径。通常这个选项无需变更，当某种特定构建环境导致模块名改变时，它仍然可使用。

#### `amdLanguageBase` 

指定用于select2语言文件解析的基础AMD加载器语言路径。通常这个选项无需变更，而当某种特定构建环境导致模块名改变时，它仍然可用。

>>> 由于r.js构建工具 [旧版本中的一个漏洞](https://github.com/jrburke/requirejs/issues/1342)，有时候在旧版本的编译构建文件中，Select2会被放在jQuery之前，导致select2无法找到或加载jQuery,因此会触发一个错误.将r.js构建工具的版本升级到2.1.18以上，可修复该问题。
