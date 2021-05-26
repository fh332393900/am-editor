---
translateHelp: true
---

# 插件

在引擎上开发一个插件是相对比较简单的。引擎提供了以下几个抽象类：

-   `Plugin` 最基础的插件抽象类
-   `ElementPlugin` 节点插件，继承自`Plugin`抽象类
-   `BlockPlugin` 块级节点插件，继承自`ElementPlugin`抽象类
-   `MarkPlugin` 样式节点插件，继承自`ElementPlugin`抽象类
-   `InlinePlugin` 行内节点插件，继承自`ElementPlugin`抽象类
-   `ListPlugin` 列表插件，继承自`BlockPlugin`抽象类

在比较复杂的插件里面，我们需要操作 DOM 树还有光标，所以仅仅继承还是不够的，我们还需要配合节点 API 来制作一个比较完善的插件。

在这里我们只需要了解有关插件的基本知识，如果需要开发插件的完整教程请在"插件"菜单里面查看。

## 使用

插件在编辑器实例化时，就会初始化插件。所以我们得在一开始就需要把插件传入引擎

```ts
const engine = new Engine(渲染节点, {
	plugins: [...插件列表],
});
```

## 命令

插件都继承自`Plugin`抽象类，必须实现 `execute` 方法。引擎会把他们加入到可执行命令列表里，并且在执行插件命令时，引擎会帮助处理好光标位置、历史记录等等

```ts
/**
 * 执行插件
 * @param args 插件需要的参数
 */
abstract execute(...args: any): void;
```

我们可以通过 `engine.command.execute("插件名称", ...插件参数)` 这种形式来执行一个插件命令

## 卡片

除了前面的无 UI 渲染的命令式和固定操作节点类型的插件外，我们还可以结合 `Card` 来完成自定义内容渲染的插件。同样的， `Card` 也是一个抽象类，我们需要继承它，它也有一个必须实现的方法 `render`（卡片渲染方法），如何渲染卡片内的节点节点完全取决于你。