---
description: 项目搭建 第二
---

# 项目搭建 第二

正如叙言中所提到的，`Blockly` 仅充当项目中的一个组件。因而搭建项目的过程，实际上是根据已有项目挑选引入 `Blockly` 类库的方式。官方教程已经讲的相当具体。此处不再赘述。

左文是官方给出的示例，包括 `Vue`、`npm`、`WebPack` 等等示例项目。

[https://github.com/google/blockly-samples/tree/master/examples](https://github.com/google/blockly-samples/tree/master/examples)

由于将 `Blockly` 嵌入项目的方式太多，不便展开，本教程不涉及项目搭建的内容。足下请按图索骥，自行查看示例项目。

本教程的示例项目基于 [blockly-webpack](https://github.com/google/blockly-samples/tree/master/examples/blockly-webpack) 修改而成。使用 `Blockly 2021Q2` 版本。

引入 `Blockly` 依赖之后，就可进行 `Blockly` 二次开发。一般而言二次开发涉及如下内容：

* 在网页内显示可视化编辑器
* 编写自己的积木外观
* 编写自己的代码生成器
* 生成自己的 `ToolBox`
* 编写序列化相关模块
* 编写或使用已有的 `Theme`
* 编写或使用其他 `Blockly` 插件（如 `Connection Checker` ）
* 通过 `MonkeyPatch` 进行其他修改（慎用）

本教程不涉及任何 `MonkeyPatch` 相关内容。使用 `MonkeyPatch` 前请先仔细研读源码。并且后果自负。
