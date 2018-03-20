---
标题: SelectAdapter
分类:
    目录: docs
---

Select2 使用`SelectAdapter` 作为`DataAdapter` 适配器的默认实现，它扩展了`BaseAdapter`。

通过将自定义适配器赋值给`dataAdapter`配置选项，重写该适配器。

 
**AMD 模块:**

- `select2/data/base`
- `select2/data/select`

## 修饰符

### `Tags`

该修饰符提供[标签](/tagging)功能。

**AMD 模块:**

`select2/data/tags`
  
### `MinimumInputLength`

该修饰符通过`minimumInputLength` 配置项提供[检索词最小长度](/searching#minimum-search-term-length) 功能。

**AMD 模块:**

`select2/data/minimumInputLength`

### `MaximumInputLength`

该修饰符通过`maximumInputLength` 配置项提供[检索词最大长度]((/searching#maximum-search-term-length)) 功能。

**AMD 模块:**

`select2/data/maximumInputLength`

### `InitSelection`

该修饰符向下兼容版本3.5的`initSelection`回调函数。

过去，每当使用自定义数据源时，Select2 都需要定义该`initSelection`配置项，从而允许确定组件的初始选择。该配置项已经被数据适配器中的`current`方法取代。 

**AMD 模块:**

`select2/compat/initSelection"`

### `Query`

该修饰符向下兼容版本3.5的`query`回调函数。

**AMD 模块:**

`select2/compat/query`

### `InputData`

该修饰符向下兼容版本3.5的`<input type="hidden" >`元素。

Select2 历史版本中，`<select>` 元素只能用于有限的选项子集。相反，还需要`<input type="hidden" >`标签，该标签不允许没用启动JavaScript的用户优雅的回退。现在Select2 支持各种配置`<select>`元素，因此也不再需要`<input />`元素。


**AMD 模块:**

`select2/compat/inputData`
