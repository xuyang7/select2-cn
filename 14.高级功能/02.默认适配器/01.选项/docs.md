---
标题: 选项
分类:
    目录: docs
---

Select2 默认提供`SingleSelection`适配器支持单选控制，和`MultipleSelection`适配器支持多选控制。`SingleSelection` 和 `MultipleSelection` 都继承了`BaseSelection`适配器。
您可以通过`selectionAdapter`配置选项重写选项配置器，实现适配器自定义。
自定义适配器

`select2/selection`

## 修饰符

### `Placeholder` and `HidePlaceholder`

**AMD 模块:**

`select2/selection/placeholder`
`select2/dropdown/hidePlaceholder`

这些修饰符提供Select2 [占位符](/placeholders)特性。

### `AllowClear`

**AMD 模块:**

`select2/selection/allowClear`


该修饰符通过`allowClear`选项提供 [选项清除](/selections#clearable-selections)功能。
### `EventRelay`

**AMD 模块:**

`select2/selection/eventRelay`

Select2 有自己的内部事件系统用于通知部分组件状态已经改变，同时也是个适配器允许将部分事件转发至系统外，供用户监听。