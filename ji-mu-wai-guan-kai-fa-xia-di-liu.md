---
description: 积木外观开发下 第六
---

# 积木外观开发下 第六

通过 `Blockly Developer Tools` 编写得到的积木仅仅只是一个骨架，这其中还有许多细节需要填充。将自动生成的 `.js` 文件导出到项目内的一个恰当位置后就需要对其进行修改。

`Blockly` 支持 `JSON` 和 `JavaScript` 两种方式去定义积木的外观。这没有版本兼容问题（新版添加的 `API` 是 `Workspace` 序列化）。然而，任何一个稍微高级一些的积木都会用到 `JavaScript` 来完成外观的变化。任何一个积木的代码生成器都采用 `JavaScript` 编写。因此本教程不通过 `Mixin` 、`Extension` 、`Mutator` 等手段来混用二者。在 `JavaScript` 定义积木外观的 `init` 函数中使用 `jsonInit` 函数进行初始化，再写 `onchange` 等函数，私以为优于使用 `Mixin` 。

因此，本教程在积木外观的基础部分中没有涉及任何具体代码。足下可以自行研究 `Blockly Developer Tools` 生成的 `JSON` 或 `JavaScript` 来摸清语法。编写两三个积木后，基础语法应该都可以掌握。

之后的内容主要针对使用纯 `JavaScript` 进行积木外观开发的场景。如果足下使用 `JSON` ，可以在官方教程内自行憭解 `Extension` 、`Mixin` 等相关内容。

### 颜色调整

在 `Block Definitions` 的代码里，可以把 `this.setColour` 方法的参数由 `HSV` 颜色改为 `#` 开头的 `RGB` 颜色。如

```javascript
 this.setColour("#681752");
```

不建议使用过亮的颜色。

### onchange 函数

所谓 `onchange` 函数，其实是一个事件监听。可以这样写：

```javascript
Blockly.Blocks['hello_world'] = {
  init: function () {
  //......  
  },
  onchange: function (event) {
  
  }
};
```

这个事件监听无法指定类型。即与此方块相关的任何事件，包括移动、拖拽等，都会传入 `event` 并调用函数。官方文档含各个事件的描述及提供的方法：

[https://developers.google.com/blockly/reference/js/Blockly.Events](https://developers.google.com/blockly/reference/js/Blockly.Events)

如需要监听拖拽事件，那么就可以这样写：

```javascript
onchange: function (event) {
    if (!this.workspace.isDragging || this.workspace.isDragging() || event.type !== Blockly.Events.BLOCK_MOVE) {
        return;    
    }    
    //...
}
```

请注意性能的消耗。很多情况下不需要使用 `onchange` 函数，因为有更好的选择。

### Field 输入检查函数

可以这样写：

```javascript
this.appendDummyInput()
     .appendField(
         new Blockly.FieldDropdown(
             [...],
             function (value) {
                 if (value) {
                 }
             }
         ),
         "TYPE");
```

其中 `value` 是用户输入的新值。这个输入检查函数在每次 `Field` 的值变化时调用。对于 `Text Input` 等其他类型的 `Field` 可以迁移使用。

在这个函数内 `this` 不是积木对象，使用 `this.getSourceBlock()` 获得积木。

### mutationToDom 和 domToMutation

这是两个用于写入和读取额外数据的函数。`Blockly` 编辑器内搭建的积木，可以序列化为 `XML` 或 `JSON` 。这两个函数专门为 `XML` 而设计。`JSON` 则使用 `saveExtraState` 等函数。本教程只涉及 `XML` 内容。

最简单的 `mutator` 如下（摘自官方教程）：

```javascript
// These are the old XML serialization hooks for the lists_create_with block.
mutationToDom: function() {
  // You *must* create a <mutation></mutation> element.
  // This element can have children.
  var container = Blockly.utils.xml.createElement('mutation');
  container.setAttribute('items', this.itemCount_);
  return container;
},

domToMutation: function(xmlElement) {
  this.itemCount_ = parseInt(xmlElement.getAttribute('items'), 10);
  // This is a helper function which adds or removes inputs from the block.
  this.updateShape_();
},
```

此处有个技巧：`domToMutation` 函数中可以直接调用 `setFieldValue` 方法，强制更新积木对象中的属性。这样在 `updateShape_` 方法中就可以直接通过 `this` 访问各种 `Field` 的值而不用担心获取不到。

尽可能通过 `mutator` 把多个积木拼合起来。如设置对象的 `X` 坐标，和获取对象的 `X` 坐标，可以做在同一个积木里。见左文进阶。
