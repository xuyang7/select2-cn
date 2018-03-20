---
标题: 下拉框
分类:
    目录: docs
---

下拉框适配器定义了下拉框在哪个主容器上。Select2 允许更改下拉菜单的工作方式，允许您从将其附加到文档中的不同位置或者添加搜索框中进行任何操作。


将修饰符与`render` and `position`方法绑定，实现改变下拉菜单的功能和位置。

通过将自定义适配器赋值给`dropdownAdapter`配置选项，重写该适配器。

`select2/dropdown`

## 修饰符

### `AttachBody`

该修饰符实现标准的下拉框添加 [`dropdownParent`](/dropdown#dropdown-placement) 方法。

**AMD 模块:**

`select2/dropdown/attachBody`

### `AttachContainer`

当加载该修饰符时，Select2 会将下拉菜单放置在容器选择之后，因此它将与Select2 其余部分DOM相同位置展示。

**AMD 模块:**

`select2/dropdown/attachContainer`

>>>> **检查你的构建.** 该模块仅用于Select2[全量构建 ](/getting-started/builds-and-modules)。

### `DropdownSearch`


该修饰符实现 [检索框在下拉框顶部显示](/searching).

**AMD 模块:**

`select2/dropdown/search`

### `MinimumResultsForSearch`

该修饰符实现 [`minimumResultsForSearch` 配置项](/searching#limiting-display-of-the-search-box-to-large-result-sets).

**AMD 模块:**

`select2/dropdown/minimumResultsForSearch`

### `CloseOnSelect`

该修饰符实现 [`closeOnSelect` 配置项](/dropdown#forcing-the-dropdown-to-remain-open-after-selection).

`select2/dropdown/closeOnSelect`
