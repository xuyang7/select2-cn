---
标题：从Select2 3.5迁移
分类:
    目录: docs
---

Select2提供了有限的后向兼容性，与之前的3.5.x发布版本兼容，允许人们更加有效地升级并获取最新功能。对于变化很大的功能变更，比如定制数据适配器工作方式的变更，会创建兼容模块辅助升级程序。不推荐依赖这些兼容模块，因为他们最终会在新发行中被移除，但是他们确实使重大变更的升级更加简单。

如果您使用Select2的完全构建（`select2.full.js`），您将会被自动通知主要的变化，并且[兼容模块](/upgrading/backwards-compatibility)会自动运行来确保您的代码实现您所预期的功能。

兼容性模块仅在Select2的[完全构建](/getting-started/builds-and-modules) 。这些文件在`.full.js`中结束，并且兼容性模块的前缀是`select2/compat`。

## 不再有隐藏输入标签

In past versions of Select2, an `<input type="hidden">` tag was recommended if you wanted to do anything advanced with Select2, such as work with remote data sources or allow users to add their own tags. This had the unfortunate side-effect of servers not receiving the data from Select2 as an array, like a standard `<select>` element does, but instead sending a string containing the comma-separated strings. The code base ended up being littered with special cases for the hidden input, and libraries using Select2 had to work around the differences it caused. 在Select2的旧版本中，如果您需要用Select2处理任何高级任务，会推荐您使用`<input type="hidden">`，比如处理远程数据源或者允许用户增加他们自己的标签。但遗憾的是这会产生对服务器的副作用，使其无法从Select中接收数据作为数组，就像标准的`<select>`元素一样，而是发送包含用逗号区分的字符串。代码库被隐藏输入的特殊案例搞得散乱，并且使用Select2的函数库不得不解决由它引起的差异。

In Select2 4.0, the `<select>` element supports all core options, and support for the old `<input type="hidden">` has been deprecated. This means that if you previously declared an AJAX field with some pre-selected options that looked like: 在Select2的4.0版本，`<select>`元素支持所有的核心选项，也支持已弃用的 `<input type="hidden">`。这意味着如果您之前用某些预选择的选项声明了一个AJAX字段，就像下面这样：

```
<input type="hidden" name="select-boxes" value="1,2,4,6" />
```

It will need to be recreated as a `<select>` element with some `<option>` tags that have `value` attributes that match the old value: 这将需要重新创建成带有某些`<option>`标签的`<select>`元素，这些元素有值属性克匹配旧值。

```
<select name="select-boxes" multiple="multiple">
    <option value="1" selected="selected">Select2</option>
    <option value="2" selected="selected">Chosen</option>
    <option value="4" selected="selected">selectize.js</option>
    <option value="6" selected="selected">typeahead.js</option>
</select>
```

The options that you create should have `selected="selected"` set so Select2 and the browser knows that they should be selected. The `value` attribute of the option should also be set to the value that will be returned from the server for the result, so Select2 can highlight it as selected in the dropdown. The text within the option should also reflect the value that should be displayed by default for the option. 您创建的选项应该具有`selected="selected"`设置，因此Sellect2和浏览器会知道应该选择它们。选项的`value`属性应该也设置成值，会从服务器返回，所以Select2能够当其在下拉菜单中被选中时进行高亮。默认条件下选项内的文本也会反映为选项显示的值，

## Advanced matching of searches 搜索的高级匹配

In past versions of Select2 the `matcher` callback processed options at every level, which limited the control that you had when displaying results, especially in cases where there was nested data. The `matcher` function was only given the individual option, even if it was a nested options, without any context. 在Select2的旧版本中，`matcher`回调在所有层次上处理选项，这会限制显示结果时的控制，尤其是有嵌入数据的时候。`matcher`函数只会分配给单个选项，即使是一个镶嵌选项，没有任何语境。

With the new [matcher function](/searching), only the root-level options are matched and matchers are expected to limit the results of any children options that they contain. This allows developers to customize how options within groups can be displayed, and modify how the results are returned. 使用新的[匹配器函数](/搜索)，只会匹配根级的选项，并且匹配器会限制它们所包含的所有子选项的结果。这就允许开发者自定义组内的选项如何显示，而且修改结果返回的方式。
 
### Wrapper for old-style `matcher` callbacks 旧版`匹配器`回调的封装器

For backwards compatibility, a wrapper function has been created that allows old-style matcher functions to be converted to the new style. 对于后向兼容，创建了一个封装器函数使得旧版匹配器函数可以转换为新型的。

This wrapper function is only bundled in the [full version of Select2](/getting-started/builds-and-modules).  You can retrieve the function from the `select2/compat/matcher` module, which should just wrap the old matcher function. 该封装器函数只捆绑在[Select2的完整版](/启动/创建-和-模块)。您可以从`select2/compat/matcher`模块中检索该函数，他应该只封装旧的匹配器函数。

<div class="s2-example">
    <select class="js-example-matcher-compat js-states form-control"></select>
</div>

<pre data-fill-from=".js-code-example-matcher-compat"></pre>

<script type="text/javascript" class="js-code-example-matcher-compat">

function matchStart (term, text) {
  if (text.toUpperCase().indexOf(term.toUpperCase()) == 0) {
    return true;
  }

  return false;
}

$.fn.select2.amd.require(['select2/compat/matcher'], function (oldMatcher) {
  $(".js-example-matcher-compat").select2({
    matcher: oldMatcher(matchStart)
  })
});

</script>

>>>> This will work for any matchers that only took in the search term and the text of the option as parameters. If your matcher relied on the third parameter containing the jQuery element representing the original `<option>` tag, then you may need to slightly change your matcher to expect the full JavaScript data object being passed in instead. You can still retrieve the jQuery element from the data object using the `data.element` property. 对于所有的只把搜索菜单中和选项的文本作为参数的匹配器，这都可以用。如果您的匹配器依赖于包含jQuery元素的第三方参数，代表着初始的`<option>`标签，那么您可能需要稍微改一下您的匹配器以期望全部JavaScript数据对象可以传递进来。您仍然可以用`data.element`属性从数据对象中检索jQuery元素。

## More flexible placeholders 更加灵活的占位符

In the most recent versions of Select2, placeholders could only be applied to the first (typically the default) option in a `<select>` if it was blank. The `placeholderOption` option was added to Select2 to allow users using the `select` tag to select a different option, typically an automatically generated option with a different value. 在Select2的最新版本中，占位符如果是空白的，仅能应用于`<select>`中第一个（通常是默认的）选项。`placeholderOption`选项增加到Select2中允许用户使用`select`标签来选择不同的选项，通常是自动生产的具有不同值的选项。

The [`placeholder` option](/placeholders) can now take an object as well as just a string. This replaces the need for the old `placeholderOption`, as now the `id` of the object can be set to the `value` attribute of the `<option>` tag. [`placeholder` option](/placeholders)现在可以处理字符串对象。这取代了对旧版`placeholderOption`的需求，因为现在对象的`id`可以设置成`<option>`标签的`value`属性。

For a select that looks like the following, where the first option (with a value of `-1`) is the placeholder option: 对于一个类似以下的程序而言，第一个选项（值为`-1`）就是占位符选项：

```
<select>
    <option value="-1" selected="selected">Select an option</option>
    <option value="1">Something else</option>
</select>
```

You would have previously had to get the placeholder option through the `placeholderOption`, but now you can do it through the `placeholder` option by setting an `id`. 之前您通过`placeholderOption`来获取占位符选项，但是现在您可以通过设置一个`id`来获取。

```
$("select").select2({
    placeholder: {
        id: "-1",
        placeholder: "Select an option"
    }
});
```

And Select2 will automatically display the placeholder when the value of the select is `-1`, which it will be by default. This does not break the old functionality of Select2 where the placeholder option was blank by default. 当选择的值为`-1`时，Select2会自动显示占位符，这都是默认的。这并不打破旧版Select2的函数性，旧版默认占位符选项是空白的。

## Display reflects the actual order of the values 显示反映了值的实际顺序

In past versions of Select2, choices were displayed in the order that they were selected. In cases where Select2 was used on a `<select>` element, the order that the server received the selections did not always match the order that the choices were displayed, resulting in confusion in situations where the order is important. 在Select2以往的版本中，选项以其被选中的顺序显示。如果Select2z在`<select>`元素上使用，服务器接收选择的顺序并不完全与显示选择的顺序匹配，导致当遇到顺序很重要的情况时，出现混乱。

Select2 will now order selected choices in the same order that will be sent to the server. 现在的Select2会保持选择的顺序和发送到服务器的顺序保持一致。

## Changed method and option names 变更的方法和选项名称。

When designing the future option set for Select2 4.0, special care was taken to ensure that the most commonly used options were brought over.  For the most part, the commonly used options of Select2 can still be referenced under their previous names, but there were some changes which have been noted. 为Select2的4.0版本设计将来选项设置时，特别注意保证最常用的功能被带过来。对于绝大部分，Select2常用的选项在它们以前的名称之下仍能够作为参考，但是还是有些变化需要注意的。

### Removed the requirement of `initSelection` 去除了`initSelection`的要求

>>>> **Deprecated in Select2 4.0.** This has been replaced by another option and is only available in the [full builds](/getting-started/builds-and-modules) of Select2. **Deprecated in Select2 4.0.**被另一个选项替代，仅在Select2的full builds](/getting-started/builds-and-modules)中可获得。

In the past, whenever you wanted to use a custom data adapter, such as AJAX or tagging, you needed to help Select2 out in determining the initial
values that were selected. This was typically done through the `initSelection` option, which took the underlying data of the input and converted it into data objects that Select2 could use. 过去，当您想要使用一个自定义的数据适配器时，比如AJAX或标签，您需要帮助Select2定义被选中的初始值。通常这通过`initSelection`选项完成，它获取输入的潜在数据并将其转换成Select2可以用的数据对象。

This is now handled by [the data adapter](/advanced/default-adapters/data) in the `current` method, which allows Select2 to convert the currently
selected values into data objects that can be displayed. The default implementation converts the text and value of `option` elements into data objects, and is probably suitable for most cases. An example of the old `initSelection` option is included below, which converts the value of the selected options into a data object with both the `id` and `text` matching the selected value. 现在这些由`current`方法中的[the data adapter](/advanced/default-adapters/data)处理，它允许Select2将当前所选值转换成可以显示的数据对象。默认应用是将`option`元素的文本和值转换成数据对象，对大多数情况都使用。一个旧的`initSelection` 选项包括在下方，它将所选选项的值转换为数据对象，既有`id`也有`text` 匹配所选择的值。

{
    initSelection : function (element, callback) {
        var data = [];
        $(element.val()).each(function () {
            data.push({id: this, text: this});
        });
        callback(data);
    }
}
```

When using the new `current` method of the custom data adapter, **this method is called any time Select2 needs a list** of the currently selected options. This is different from the old `initSelection` in that it was only called once, so it could suffer from being relatively slow to process the data (such as from a remote data source). 当使用定制数据适配器的新`current`方法时，**该方法在Select2需要当前所选选项的清单时会被调用**。这和旧版的`initSelection` 不同，在旧版中它只会调用一次，所以后果就是处理数据时会相对较慢（比如处理来自远程数据源的数据时）

```
$.fn.select2.amd.require([
    'select2/data/array',
    'select2/utils'
], function (ArrayData, Utils) {
    function CustomData ($element, options) {
        CustomData.__super__.constructor.call(this, $element, options);
    }

    Utils.Extend(CustomData, ArrayData);

    CustomData.prototype.current = function (callback) {
        var data = [];
        var currentVal = this.$element.val();

        if (!this.$element.prop('multiple')) {
            currentVal = [currentVal];
        }

        for (var v = 0; v < currentVal.length; v++) {
            data.push({
                id: currentVal[v],
                text: currentVal[v]
            });
        }

        callback(data);
    };

    $("#select").select2({
        dataAdapter: CustomData
    });
}
```

The new `current` method of the data adapter works in a similar way to the old `initSelection` method, with three notable differences. The first, and most important, is that **it is called whenever the current selections are needed** to ensure that Select2 is always displaying the most accurate and up to date data. No matter what type of element Select2 is attached to, whether it supports a single or multiple selections, the data passed to the callback **must be an array, even if it contains one selection**.数据适配器的新`current`方法与旧版`initSelection`工作方法类似，但有3处明显的差异。第一，也是最重要的，无论何时需要当前选择，它都会被调用，以此来保证Select2总是在显示最新最准确的数据。无论Select2被附加于什么元素类型，无论它是否支持单选还是多选，传递给回调的数据**必须要死一个数组，即使它只包含一个选择。

The last is that there is only one parameter, the callback to be executed with the latest data, and the current element that Select2 is attached to is available on the class itself as `this.$element`. 最后一点是只有一个参数，用最新的数据执行回调，Select2附加于的当前元素在其自身作为`this.$element`的类上是可获取的。

If you only need to load in the initial options once, and otherwise will be letting Select2 handle the state of the selections, you don't need to use a custom data adapter. You can just create the `<option>` tags on your own, and Select2 will pick up the changes.如果您只需要在初始选项中加载一次，而在其他情况下让Select2处理选择的状态，那么您就不需要使用定制数据适配器。您可以自行创建`<option>` 标签，然后Select2会识别这个变化。

```
var $element = $('select').select2(); // the select element you are working with 您正在处理的选择元素

var $request = $.ajax({
    url: '/my/remote/source' // wherever your data is actually coming from无论你的数据实际来自何处
});

$request.then(function (data) {
    // This assumes that the data comes back as an array of data objects这里假设数据作为数据对象数组返回
    // The idea is that you are using the same callback as the old  `initSelection`

    for (var d = 0; d < data.length; d++) {
        var item = data[d];

        // Create the DOM option that is pre-selected by default
        var option = new Option(item.text, item.id, true, true);

        // Append it to the select
        $element.append(option);
    }

    // Update the selected options that are displayed
    $element.trigger('change');
});
```

### Custom data adapters instead of `query`

>>>> **Deprecated in Select2 4.0.** This has been replaced by another option and is only available in the [full builds](/getting-started/builds-and-modules) of Select2.

[In the past](http://select2.github.io/select2/#data), any time you wanted to hook Select2 up to a different data source you would be required to implement custom `query` and `initSelection` methods. This allowed Select2 to determine the initial selection and the list of results to display, and it would handle everything else internally, which was fine more most people.

The custom `query` and `initSelection` methods have been replaced by [custom data adapters](/advanced/default-adapters/data) that handle how Select2 stores and retrieves the data that will be displayed to the user. An example of the old `query` option is provided below, which is
[the same as the old example](http://select2.github.io/select2/#data), and it generates results that contain the search term repeated a certain number of times.

```
{
    query: function (query) {
        var data = {results: []}, i, j, s;
        for (i = 1; i < 5; i++) {
            s = "";
            for (j = 0; j < i; j++) {
                s = s + query.term;
            }
            data.results.push({
                id: query.term + i,
                text: s
            });
        }
        query.callback(data);
    }
}
```
This has been replaced by custom data adapters which define a similarly named `query` method. The comparable data adapter is provided below as an example.

```
$.fn.select2.amd.require([
'select2/data/array',
'select2/utils'
], function (ArrayData, Utils) {
    function CustomData ($element, options) {
        CustomData.__super__.constructor.call(this, $element, options);
    }

    Utils.Extend(CustomData, ArrayData);

    CustomData.prototype.query = function (params, callback) {
        var data = {
            results: []
        };

        for (var i = 1; i < 5; i++) {
            var s = "";

            for (var j = 0; j < i; j++) {
                s = s + params.term;
            }

            data.results.push({
                id: params.term + i,
                text: s
            });
        }

        callback(data);
    };

    $("#select").select2({
        dataAdapter: CustomData
    });
}
```

The new `query` method of the data adapter is very similar to the old `query` option that was passed into Select2 when initializing it. The old `query` argument is mostly the same as the new `params` that are passed in to query on, and the callback that should be used to return the results is now passed in as the second parameter.

### Renamed templating options

Select2 previously provided multiple options for formatting the results list and selected options, commonly referred to as "formatters", using the `formatSelection` and `formatResult` options. As the "formatters" were also used for things such as localization, [which has also changed](#renamed-translation-options), they have been renamed to `templateSelection` and `templateResult` and their signatures have changed as well.

You should refer to the updated documentation on templates for [results](/dropdown) and [selections](/selections) when migrating from previous versions of Select2.

### The `id` and `text` properties are strictly enforced

When working with array and AJAX data in the past, Select2 allowed a custom `id` function or attribute to be set in various places, ranging from the initialization of Select2 to when the remote data was being returned. This allowed Select2 to better integrate with existing data sources that did not necessarily use the `id` attribute to indicate the unique identifier for an object.

Select2 no longer supports a custom `id` or `text` to be used, but provides integration points for converting to the expected format:

#### When working with array data

Select2 previously supported defining array data as an object that matched the signature of an AJAX response. A `text` property could be specified that would map the given property to the `text` property on the individual objects. You can now do this when initializing Select2 by using the following jQuery code to map the old `text` and `id` properties to the new ones.

```
var data = $.map([
    {
        pk: 1,
        word: 'one'
    },
    {
        pk: 2,
        word: 'two'
    }
], function (obj) {
    obj.id = obj.id || obj.pk;
    obj.text = obj.text || obj.word;

    return obj;
});
```

This will result in an array of data objects that have the `id` properties that match the existing `pk` properties and `text` properties that match the existing `word` properties.

#### When working with remote data

The same code that was given above can be used in the `processResults` method of an AJAX call to map properties there as well.

### Renamed translation options

In previous versions of Select2, the default messages provided to users could be localized to fit the language of the website that it was being used on. Select2 only comes with the English language by default, but provides [community-contributed translations](/i18n) for many common languages. Many of the formatters have been moved to the `language` option and the signatures of the formatters have been changed to handle future additions.

### Declaring options using `data-*` attributes

In the past, Select2 has only supported declaring a subset of options using `data-*` attributes. Select2 now supports declaring all options using the attributes, using [the format specified in the documentation](/configuration/data-attributes).

You could previously declare the URL that was used for AJAX requests using the `data-ajax-url` attribute. While Select2 still allows for this, the new attribute that should be used is the `data-ajax--url` attribute. Support for the old attribute will be removed in Select2 4.1.

Although it was not documented, a list of possible tags could also be provided using the `data-select2-tags` attribute and passing in a JSON-formatted array of objects for tags. As the method for specifying tags has changed in 4.0, you should now provide the array of objects using the `data-data` attribute, which maps to [the array data](/data-sources/arrays) option. You should also enable tags by setting `data-tags="true"` on the object, to maintain the ability for users to create their own options as well.

If you previously declared the list of tags as:

```
<select data-select2-tags='[{"id": "1", "text": "One"}, {"id": "2", "text": "Two"}]'></select>
```

...然后你必须按如下声明...

```
<select data-data='[{"id": "1", "text": "One"}, {"id": "2", "text": "Two"}]' data-tags="true"></select>
```

## 弃用和移除方法

由于现在Select2 对所有数据源都使用`<select>`元素，因此之前调用`.select2()`的一些方法将不再需要。
### `.select2("val")`

弃用`"val"` 方法，Select2 4.1版本将移除该方法。弃用的方法将不再需要包含`triggerChange`参数。相反，你必须在底层 `<select>` 元素上直接调用`.val` ，如果你需要 (`triggerChange`)的第二个参数， (`triggerChange`), 同时你还需要调用 `.trigger("change")` 。

```
$("select").val("1").trigger("change"); // 替换 $("select").select2("val", "1");
```

### `.select2("enable")`

Select2 仍然会识别底层select元素的`disabled` 属性。 您可以在`<select>` 元素上调用`.prop('disabled', true/false)` 方法启用和禁用Select2。Select2 4.1 版本将彻底移除旧方法，不再支持。

```
$("select").prop("disabled", true); // 替换 $("select").enable(false);
```
