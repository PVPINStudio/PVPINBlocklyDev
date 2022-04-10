---
description: 项目搭建 第二
---

# 项目搭建 第二

正如叙言所说，`Blockly` 仅充当项目中的一个组件。因而虽谓搭建项目，实则是根据已有项目挑选恰当方式引入 `Blockly` 类库。

下有官方示例，包括 `Vue`、`npm`、`WebPack` 等等示例项目。

[https://github.com/google/blockly-samples/tree/master/examples](https://github.com/google/blockly-samples/tree/master/examples)

由于将 `Blockly` 嵌入项目的方式实在太多，此处不便展开，故不再赘述。足下请按图索骥，自行查看示例项目，并引入 `Blockly`。建议能直接采用官方写法处不要随意修改。如在使用 `webpack` 时，只引入 `blockly/core` 会导致无法加载原生积木，这就是没有采用官方示例写法导致的。

本教程的示例项目基于 [blockly-webpack](https://github.com/google/blockly-samples/tree/master/examples/blockly-webpack) 修改而成。使用 `Blockly 2021Q2` 版本。

引入 `Blockly` 依赖之后，就可进行 `Blockly` 二次开发。一般而言二次开发涉及如下内容：

* 在网页内显示 `Blockly` 编辑器
* 编写自己的积木外观
* 编写自己的积木代码生成器
* 生成自己的 `ToolBox`
* 编写序列化相关模块
* 编写或使用已有的 `Theme`
* 编写或使用其他 `Blockly` 插件（如 `Connection Checker` ）
* 通过 `MonkeyPatch` 进行其他修改（慎用）

本教程不会涉及任何 `MonkeyPatch` 相关内容。使用 `MonkeyPatch` 前请先仔细研读源码。后果自负。
