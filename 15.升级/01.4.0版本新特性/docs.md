---
标题: 4.0 有那些新特性
分类:
    目录: docs
---

我们研究了代码库，确立方向后，三年磨一剑，Select2 4.0 版本最终发行。几乎重构了整个核心逻辑，从而解决了很多历史版本无法解决的可扩展性和可用性问题。


这次发行有很多突破性的变化，但是创建了易升级路径，还有帮助模块，允许后向兼容，可维护Select2的以往版本。升级过程**将**需要你仔细阅读发布说明，但是迁移路径会相对简单。您可以在[发布说明](https://github.com/select2/select2/releases)中查看最常见的变更清单，这些变更也是您需要做的。

这个新版本包含的显著特点如下：

- 更灵活的插件框架，允许您重写Select2以准确地执行您想要的操作。
- 所有数据适配器与标准的select 元素保持一致，移除隐藏的`<input>`元素。
- 使用AMD构建系统保证一切有序。
- 允许Select2易适应您的应用程序的其他部分。

## 插件系统

Select2现在提供了 接口一边可轻松扩展，允许任何人创建插件来改变Select2的工作方式。这是由于Select2被分成3个独立的部分，每个部分都可以扩展，也可组合使用来创建属于您独一无二的Select2.

适配器采用了一致性接口，在[进阶篇](/advanced/adapters-and-decorators)中可以看到，允许根据自定义需求对重新定义Select2。Select2就是为了让您能够混用并匹配插件而设计的，大部分核心选项都作为修饰符而创建，对标准适配器进行封装。

## 基于AMD构建系统

Select2 使用[基于AMD 构建系统](https://en.wikipedia.org/wiki/Asynchronous_module_definition)，允许按需部分构建。尽管Select2无法创建自定义构建系统，但Select2是开源的，所以支持pull 请求拉取源代码后自定义构建。

Select2 包含最小 [almond](https://github.com/jrburke/almond) AMD 加载器,
如果您的系统中已经使用AMD加载，仍也可以使用`select2.amd.js`构建。基线代码库 (`src` 目录)同样使用AMD，允许您在自己的构建系统中使用Select2，并在已有的基础架构上生成您自己的构建。 

Select2 使用的AMD 方法可用于 `jQuery.fn.select2.amd.define()/require()`, 并且允许您使用已包含的 almond 加载器。这些方法主要用于翻译功能，但它们是访问Select2提供自定义模块的推荐方法。