---
description: 连接检查插件开发 第十
---

# 连接检查插件开发 第十

如果需要对 `Blockly` 的行为进行自定义，最佳选择是使用插件。插件可以调用 `Blockly` 的各种 `API` ，也可以用于实现一些 `Blockly` 提供的接口。本章偏重后者。

官方提供了一些插件，不过官方插件的一条准绳是普适性强，自然限制其针对性。如果需要搭建一个功能完备的网站或应用程序，难免需要自定义插件。

## 添加插件

对于一个实现 `Blockly` 接口的插件而言，必须进行注册才能让 `Blockly` 使用插件的实现类。此处以连接检查的类为例，在注入时指定 `connectionChecker`：

```javascript
var options = {
  ...
  plugins: {
    'connectionChecker': 'PVPINConnectionChecker'
  }
};
const workspace = Blockly.inject('blocklyDiv', options);
```

之后写一个类实现 `ConnectionChecker` ：

```javascript

/**
 * @implements {Blockly.IConnectionChecker}
 */
class PVPINConnectionChecker extends Blockly.ConnectionChecker {
    constructor() {
        super();
    }

    /**
     * Type check arrays must either intersect or both be null.
     * @override
     */
    doTypeChecks(a, b) {
        const checkArrayOne = a.getCheck();
        // Output
        const checkArrayTwo = b.getCheck();
        // Input
        return this.doChecks_(checkArrayOne, checkArrayTwo);
    }

    doChecks_(checkArrayOne, checkArrayTwo) {
        if (!checkArrayOne || !checkArrayTwo) {
            // One is null, permit connection.
            return true;
        }
        // Find any intersection in the check lists.
        for (let i = 0; i < checkArrayOne.length; i++) {
            for (let j = 0; j < checkArrayTwo.length; j++) {
                if (checkArrayOne[i] == checkArrayTwo[j]) {
                    return true;
                }
            }
        }
        // ...
        return false;
    }
}

const registrationType = Blockly.registry.Type.CONNECTION_CHECKER;
const registrationName = "PVPINConnectionChecker";

// Register the checker so that it can be used by name.
Blockly.registry.register(registrationType, registrationName, PVPINConnectionChecker);

const pluginInfo = {
    [registrationType]: registrationName,
};

```

上方的插件，实际上就是原生连接检查，仅做出微小的修改。首先，读取 `Input` 和 `Output` 类型。如果任一个为空则允许连接。如果均非空，再判断两个类型数组是不是有重叠项，如 `["a", "b"]` 和 `["b", "c"]` 可以连接。

通过恰当连接检查可以模拟如 `Java` 多态这样的语法。这样做需要首先在 `Java` 环境内运行一段程序，把需要的类的所有超类类名进行适当的储存。再在运行时恰当读取。

在 `doChecks_` 内添加：

```javascript
        for (let i = 0; i < checkArrayOne.length; i++) {
            for (let j = 0; j < checkArrayTwo.length; j++) {
                if (!MAP[checkArrayOne[i]]) {
                    return false;
                }
                if (MAP[checkArrayOne[i]].indexOf(checkArrayTwo[j]) > -1) {
                    return true;
                }
            }
        }
```

再生成依赖的集合，如：

```javascript
const MAP = {
    "java.lang.String": [
        "java.lang.CharSequence",
        // ......
    ],
    // ......
}
```

那么能连接 `CharSequence` 处都可以连接 `String` 了，使得设计积木时可以更好地按照  `Java` 多态原先的面貌设计。

完整代码见 BBS 回复可见内内容。

## 可用的接口

[官方教程](https://developers.google.cn/blockly/guides/plugins/using_blockly_apis) 已经阐明只有七个接口可供实现。它们各有注册名，如 `connectionChecker`，也有类名，如 `IConnectionChecker`。其他功能没有接口。

慎用 `MonkeyPatch`。本教程不会演示。
