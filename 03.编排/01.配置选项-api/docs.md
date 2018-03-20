---
标题: 编排 API
分类:
    目录: docs
---

Select2 所有配置项的综合列表，如下：

| 选项 | 类型 | 默认值 | 描述 | 
| ------ | ---- | ------- | ----------- |
| `adaptContainerCssClass` | | | |
| `adaptDropdownCssClass` | | | |
| `ajax` | object | `null` | 支持 [Ajax 数据源](/data-sources/ajax)。 |
| `allowClear` | boolean | `false` | 支持 [选项清除 ](/selections#clearable-selections)。 |
| `amdBase` | string | `./` | 详见 [AMD 和 CommonJS 加载 Select2](/builds-and-modules#using-select2-with-amd-or-commonjs-loaders)。 |
| `amdLanguageBase` | string | `./i18n/` | 详见 [AMD 和 CommonJS 加载 Select2](/builds-and-modules#using-select2-with-amd-or-commonjs-loaders)。 |
| `closeOnSelect` | boolean | `true` | 选择结束后，[是否关闭下拉菜单 ](/dropdown#forcing-the-dropdown-to-remain-open-after-selection)。 |
| `containerCss` | | | |
| `containerCssClass` | string | `''` | |
| `data` | array of objects | `null` | 允许使用 [数组](/data-sources/arrays) 渲染下拉列表。 |
| `dataAdapter` | | `SelectAdapter` | 重写内置 [DataAdapter](/advanced/default-adapters/data)。 |
| `debug` | boolean | `false` | 允许在浏览器控制台中调试。 |
| `dir` | | |
| `disabled` | boolean | `false` | 置为`true`时，禁用选择控制。 |
| `dropdownAdapter` | | `DropdownAdapter` | 重写内置 [DropdownAdapter](/advanced/default-adapters/dropdown)。 |
| `dropdownAutoWidth` | boolean | `false` | |
| `dropdownCss` | | | |
| `dropdownCssClass` | string | `''` | |
| `dropdownParent` | jQuery selector or DOM node | `$(document.body)` | 允许自定义 [下拉菜单的位置](/dropdown#dropdown-placement)。 |
| `escapeMarkup` | callback | `Utils.escapeMarkup` | 处理 [通过自动以模板渲染的自定义转义](/dropdown#built-in-escaping). |
| `initSelection` | callback | |详见 [`initSelection`](/upgrading/migrating-from-35#removed-the-requirement-of-initselection).  **v4.0已弃用，将在v4.1中删除。** |
| `language` | string or object | `EnglishTranslation` | 指定用于 [Select2 消息的语言](/i18n#message-translations). |
| `matcher` | A callback taking search `params` and the `data` object. | | 处理自定义[搜索匹配](/searching#customizing-how-results-are-matched). |
| `maximumInputLength` | integer | `0` |搜索框中允许检索的 [最大字符数](/searching#maximum-search-term-length)。|
| `maximumSelectionLength` | integer | `0` | 多选框中最多允许选择几项，如果小于1，则没限制。
| `minimumInputLength` | integer | `0` | [搜索最小字符数](/searching#minimum-search-term-length) |
| `minimumResultsForSearch` | integer | `0` | [显示搜索框](/searching#limiting-display-of-the-search-box-to-large-result-sets)所需的最小结果集。|
| `multiple` | boolean | `false` | 多选框(pillbox)启用参数，Select2 自动用`multi` HTML 属性值初始化下拉项。 |
| `placeholder` | string or object | `null` | 指定控件[占位符](/placeholders).|
| `query` | A function taking `params` (including a `callback`) | `Query` | **Select2 v4.0已弃用，将在v4.1中删除。** |
| `resultsAdapter` | | `ResultsAdapter` | 重写内置 [ResultsAdapter](/advanced/default-adapters/results). |
| `selectionAdapter` | | `SingleSelection` or `MultipleSelection`, depending on the value of `multiple`. | 重写内置 [SelectionAdapter](/advanced/default-adapters/selection). |
| `selectOnClose` | boolean | `false` | 下拉框关闭时[自动选择 ](/dropdown#automatic-selection)|
| `sorter` | callback | | |
| `tags` | boolean / array of objects | `false` | 是否启用[自由文本响应](/tagging). |
| `templateResult` | callback | | 自定义[搜索结果渲染方式](/dropdown#templating). |
| `templateSelection` | callback | | 自定义[选项渲染方式](/selections#templating). |
| `theme` | string | `default` | 允许自定义 [主题](/appearance#themes). |
| `tokenizer` | callback | | 回调函数， 用于自动化处理 [自由文本标记](/tagging#automatic-tokenization-into-tags). |
| `tokenSeparators` | array | `[]` | 可用作令牌分隔符的字符列表. |
| `width` | string | `resolve` | 支持 [容器宽度自定义](/appearance#container-width). |
