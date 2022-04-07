---
description: 积木外观开发上 第五
---

# 积木外观开发上 第五

右文提到了如何注入 `WorkSpace` ，在注入后就需要添加自己的积木。添加积木的过程分为三步：首先定义积木外观，再完成代码生成器，最后将积木置于 `ToolBox` 中。

### Work Space 的结构

![WorkSpace](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-1.png?lastModify=1649341862)

以上是一个完整的 `WorkSpace` 。左侧包含了 逻辑、循环 等积木类别的竖栏称为 `ToolBox` 。点击 `ToolBox` 之后在右侧显示的，供用户选取积木的部分称为 `FlyOut` 。右侧搭建积木的空白区域和左侧 `ToolBox` 、`FlyOut` 共同组成 `WorkSpace` 。

`ToolBox` 划分为多个 `Category` 。逻辑、循环等是 `Google Blockly` 自带的原生 `Category` 。变量、函数是较为特殊的 `Category` ，因为它们包含的积木是可变的（添加多个变量或函数后，`Category` 的内容发生改变）。在“颜色”和“变量”之间有一道空隙将二者隔开，称之为 `Separator` 。

### 积木外观开发方法

`Blockly Developer Tools` 不仅能用来生成注入 `WorkSpace` 时的配置项，还提供了开发积木外观的方法，并预设好了 `Code Generator` 代码的框架。

官方教程：[https://developers.google.com/blockly/guides/create-custom-blocks/define-blocks](https://developers.google.com/blockly/guides/create-custom-blocks/define-blocks)

`Developer Tools` ：[https://developers.google.com/blockly/guides/create-custom-blocks/blockly-developer-tools](https://developers.google.com/blockly/guides/create-custom-blocks/blockly-developer-tools)

你可以在 `Block Factory` 中自由搭建积木外观，通过搭积木的方式你可以轻松搞定基础积木开发语法。

![Stmt](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-2.png?lastModify=1649341862)![Stmt](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-3.png?lastModify=1649341862)

上方的方块被称为 `Reporter Block` ，特点是输出一个返回值。这一说法出自 `MicroSoft PXT` 文档。`Blockly` 本身并没有这一名词，一般直接称该积木拥有 `Output` 。

![Reporter](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-4.png?lastModify=1649341862)![Reporter](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-5.png?lastModify=1649341862)

上方的方块被称为 `Statement Block` ，特点是没有返回值，可以看作是 `void` 函数，没有输出。

![inline](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-6.png?lastModify=1649341862)![inline](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-7.png?lastModify=1649341862)

![External](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-8.png?lastModify=1649341862)![External](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-9.png?lastModify=1649341862)

`Inputs` 规定输入是内联还是外联，单行尽可能内联，多行可以外联。

具体效果见上，`external` 为外联（第一个），`inline` 为内联（第二个）。

![Top\&Bottom](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-10.png?lastModify=1649341862)![Top](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-11.png?lastModify=1649341862)

连接形式，如为 `Reporter`，请使用 `left output` 。

如为 `Statement` 请使用 `top bottom connection` ，这样积木上下两侧都可以连接其他积木。

如果你想写的是“终止循环”或者“返回值”这类积木，那么积木下方不需要连接其他积木，也就不需要底部的 连接。（直接跳出循环，那么这块积木底下的代码当然没有任何意义了，返回意味着函数的这个逻辑分支到终点了，因此直接拒绝在下方连接任何积木）

其实上下方连接都可以指定连接类型，不过默认不使用。

![ToolTip](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-12.png?lastModify=1649341862)

`tooltip` 规定鼠标悬停时的描述，可以对功能进行中文翻译。

![HelpURL](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-13.png?lastModify=1649341862)

`help url` 规定跳转的网站。（右键积木，点击 `Help` 后跳转）

`colour` 规定积木颜色。左文将会再次提及颜色的修改。

之后，你可以从左侧 `input` 处选取输入。

![InputValue](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-14.png?lastModify=1649341862)

![InputStmt](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-15.png?lastModify=1649341862)

![InputDummy](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-16.png?lastModify=1649341862)

上面展示的就是三种 `Input` 形式。

其中 `Value Input` 为接受指定类型 `Reporter` 积木的输入。类型可以留空，可以是一种，也可以是一个列表。默认的连接处理方式是：`output` 和 `input` 任意一项为空则允许连接，如果二者均非空，则判断是否有重合，如有则允许连接。当然，连接处理是可以通过插件进行自定义的。见左文。

`Statement Input` 为接受任意 `Statement` 的一种输入。

`Dummy Input` 相当于没有任何输入，仅仅只是可以用来加 `Field` 或者文字。

![Dropdown](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-17.png?lastModify=1649341862)

以上是一个 `Dropdown Field`，可以看作是一个返回值对用户无意义的枚举。（用户只关心下拉后的功能，下拉的结果没有意义）。

需要注意，请尽可能少用 `Field` 中的 `Text Input` 或 `Numeric Input` ，应当直接使用 `Value Input` 。

以 `Text Input` 为例，它仅仅接受用户直接将字符串输入一个框中，但是如果字符串是一个变量，或字符串是由 `Reporter` 积木所返回的，就无法嵌入这个框当中，如下所示。同理，少用直接的数字输入。但是可以使用 `CheckBox` 和颜色输入（ `RGB` ）。

![Text Input](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-18.png?lastModify=1649341862)

### 积木导出

导出积木时：

第一步，切换到 `Block Exporter` 。（别忘了先按 `Save` 保存现有积木）

第二步，把 `Block Definitions` 和 `Generator` 全部切换为 `JavaScript` 。

![Exporter](file:///C:/Users/williamshi/Documents/Code/PVPIN/Tutorial/PVPINBlocklyDev/img/5-19.png?lastModify=1649341862)

第三步，把 `Block Definitions` 和 `Generator` 的全部内容都黏贴进某个 `.js` 文件中。

第四步，切换到 `Block Factory` ，并 `DownLoad Block Library` 。

可以这样理解，在 `Developer Tools` 里写的积木，都是以 `XML` 的形式保存的，一定程度上相当于源代码。将源代码编译产生的是 `.js` 文件。为应对下次再导入修改的情况，建议导出 `XML` 。

在示例项目中将不会包括导出的 `XML` 。示例积木在 `customblocks.js` 内。

### 积木导入

第一步，找到 `XML` 文件。

第二步，上传到 `Blockly Developer Tools`（右侧 `Import Block Library` ）。

第三步，按左侧 `Block Library`，`Create New Block` 。然后进行开发。
