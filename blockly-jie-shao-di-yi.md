---
description: Blockly 介绍 第一
---

# Blockly 介绍 第一

每一个季度，`Blockly` 会发布一个大版本。春季（约三月）发布的是 `Q1` ，夏季（约六月）发布的是 `Q2` ，秋季（约九月）发布的是 `Q3` ，冬季（约十二月）发布的是 `Q4` 。在季度前加上年份就是完整的版本号。如 2021 年夏季发布的是 `2021Q2` 。左文（竖排版古籍中书向左翻开，左文即下文）将全部采用年份加季度的版本号。

`Blockly 2021Q3` 和 `Q4` 都属于过渡版本。目前，`Blockly` 正在进行模块化，并筹备从 `JavaScript` 逐步迁移到 `TypeScript`，但此过程更改所有核心代码，仍未完工。`Q4` 版本仍有模块化引起的已知问题（比如 `loopTypes` 无法添加自定义积木 `ID` ）。并且维护者称将在 `2022Q1` 修复（参见 Issue#5819）。故建议开发者先基于 `2021Q2` 掌握一些通用 `API` ，再转而使用 `2022Q1` 的新 `API` 。不建议贸然采用 `Q3` 或 `Q4` 版本。（本章于 2022.2 写就，时 `2022Q1` 仍未发布）

`2021Q2` 到 `2022Q1` 最主要的 `API` 更新是增加 `JSON` 序列化。见 PR#5487。本教程主要使用 `XML` 序列化。虽说官方文档已经更新了关于 `JSON` 序列化的部分，但其他配套项目如 `Blockly Developer Tools` 等还是没有增加导出为 `JSON` 的功能。本教程日后可能补加相关部分。

本教程参考引证的文章、项目如下：

`Blockly` 官方教程（[https://developers.google.com/blockly/guides/overview](https://developers.google.com/blockly/guides/overview)）

`Blockly` 开源仓库（[https://github.com/google/blockly](https://github.com/google/blockly)）

`Blockly` 官方示例开源仓库（[https://github.com/google/blockly-samples](https://github.com/google/blockly-samples)）

`Blockly` 官方示例页面（[https://blockly-demo.appspot.com/static/demos/index.html](https://blockly-demo.appspot.com/static/demos/index.html)）

`MicroSoft PXT` 文档（[https://makecode.com/docs](https://makecode.com/docs)）

`Code Lab` （[https://blocklycodelabs.dev/](https://blocklycodelabs.dev)）

`Blockly` 官方论坛（[https://groups.google.com/forum/#!forum/blockly](https://groups.google.com/forum/#!forum/blockly)）

`StackOverFlow` 上有一些相关回答可供参考。

`Blockly` 提供了 8 大类 56 块原生积木。其源码有重要学习和借鉴价值。

\
