---
description: 积木代码生成器开发 第七
---

# 积木代码生成器开发 第七

### 专有名词解释

所有的积木全部分为两个部分。一是外观部分，定义了积木上的文字，输入与输出，以及可能的外观变形。二是代码生成器部分，定义了积木所对应的代码。代码生成器称之为 `Code Generator` 。

所有的积木理论上都可以有多个代码生成器，`Blockly` 自带了 `JavaScript Lua Dart PHP Python` 这五种语言的生成器。本教程只演示 `JavaScript` 代码生成器的使用。也可以注册新语言，不过本教程不会提及。

### 预设的 Generator 代码

事实上，当我们定义积木外观时，就已经得到了一个预设好的 `Generator` 。

这个预设的 `Generator` 很有用。

![Generator](.gitbook/assets/7-1.png)

如图的红框就是预设的 `Generator` 。

### Generator 访问输入值

首先，输入值有两种。

![Value](.gitbook/assets/7-2.png)

![Field](.gitbook/assets/7-3.png)

第一张图是一个 `Value Input` 。第二张图是一个 `Field Input`。

其区别在于，第一张图的输入可以是一串有返回值的方法，而第二张图必须由用户在浏览器内打字输入。

![Multi Inputs](.gitbook/assets/7-4.png)

事实上，一个积木可以有很多个输入。为了区别这些输入，每一个输入都要有一个 `Name` 作为标记。通过 `Name` 可以访问这个输入的值。

![Default](.gitbook/assets/7-5.png)

这个输入的值，已经写在了预设代码里。照抄即可。

事实上，预设代码会生成所有获取 `Field Input`、`Value Input`、`DropDown` 的值的代码。只需要调用这些变量。最终拼接为一个字符串即可。`Statement` 积木需要在末尾加上  。

### 正确使用自定义函数

有一个所谓缓存参数的概念，实质上是使用了自定义函数功能。自定义函数即首先在文件的最开头加上一段函数 `function(...) {...}` 。可以在最终返回的代码里调用这个函数。如原生积木中的混合颜色积木，就提供了 `colorBlend` 函数。

使用函数的好处主要在于可以在 `Reporter` 积木中进行一些 `Statement` 调用和缓存参数。

比如要使用 `Java` 中的 `INetSocketAddress` 类型的对象获取 `IP` 字符串，就可以这么写：

```
var functionName = Blockly.JavaScript.provideFunction_("iNetSocketAddress_toString", [    "function " + Blockly.JavaScript.FUNCTION_NAME_PLACEHOLDER_ + "(iNetSocketAddress) {",    "  var hostStr = iNetSocketAddress.getHostString();",    "  var portStr = iNetSocketAddress.getPort();",    "  var ret = hostStr + ':' + portStr;",    "  return ret;",    "}",]);code = functionName + "(" + ... + ")";
```

另外一个好处在于，同一个 `INetSocketAddress` 类型的对象可以多次复用。假设这个对象是调用某个耗时方法得到的（即省略号位置处是一个对函数的调用），那么缓存参数可以提高效率。假设这个对象和随机有关，如生成随机长度列表，那么不缓存会造成错误。

### 全部代码的生成

将 `workspace` 内积木转换为 `JavaScript` 代码，只需要这样写：

```
 Blockly['JavaScript'].workspaceToCode(workspace);
```

可以使用非原生支持的语言，但是必须为所有用到的积木都写代码生成器。
