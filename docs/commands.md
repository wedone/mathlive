# 命令

您可以通过编程方式对 mathfield 执行编辑操作。例如，在用户点击按钮时插入一个分数。

您可以通过向 mathfield 发送**命令**来实现，例如 `"select-all"`、`"move-to-next-char"`、`"delete-backward"` 等。

**要发送命令**，使用 [`mf.executeCommand()`](https://mathlive.io/docs/mathfield/#(%22mathfield-element%22%3Amodule).(MathfieldElement%3Aclass).(executeCommand%3Ainstance)) 方法。

```js
mf.executeCommand("delete-backward");
```

**要将命令与虚拟键盘按键关联**，在按键定义中使用 `command` 属性。例如：

```js
{
  "class": "action",
  "label": "Delete",
  "command": 'perform-with-feedback("delete-backward")'
}
```

命令由一个称为**选择器（selector）** 的字符串标识。

选择器可以使用大驼峰（CamelCase）或连字符（kebab-case）语法。例如：`"moveToNextChar"` 和 `"move-to-next-char"` 是同一个选择器。

大多数命令不带参数。当命令有参数时，可以将选择器和命令参数组成的元组传递给 `executeCommand()`。例如：

```js
mf.executeCommand(["insert", "(#0)"]);
```

上面的命令将在选区周围插入一对括号（`#0` 序列会被替换为当前选区）。

---

## 编辑命令

- **`insert`**：此选择器接受两个参数。第一个参数是必需的，即要插入的内容（字符串）。第二个参数是可选的键值对集合：
  - `insertionMode`：可选值为 `"replaceSelection"`（替换选区）、`"replaceAll"`（替换全部）、`"insertBefore"`（在之前插入）或 `"insertAfter"`（在之后插入）。
  - `selectionMode`：可选值为 `"placeholder"`（选区将位于插入项中第一个可用的占位符处）、`"after"`（选区将位于插入项之后）、`"before"`（选区将位于插入项之前）或 `"item"`（将选中插入的项）。

- **`delete`**：`deleteNextChar` 的同义词

- **`deleteBackward`** / **`deleteForward`**：向后/向前删除

- **`deleteNextWord`** / **`deletePreviousWord`**：删除下一个/上一个单词

- **`deleteToGroupStart`** / **`deleteToGroupEnd`**：删除到组开头/结尾

- **`deleteToMathFieldEnd`**：删除到 mathfield 末尾

- **`deleteAll`**：全部删除

- **`transpose`**：转置

---

## 编辑菜单

- **`undo`**：撤销
- **`redo`**：重做
- **`cutToClipboard`**：剪切到剪贴板
- **`copyToClipboard`**：复制到剪贴板
- **`pasteFromClipboard`**：从剪贴板粘贴

---

## 用户界面

- **`commit`**：用户已完成输入。按下 RETURN 或 ENTER 键时触发。
- **`switchMode`**：切换模式
- **`complete`**：退出命令模式并插入结果
- **`nextSuggestion`** 和 **`previousSuggestion`**：当弹出面板被选中时，显示下一个/上一个建议
- **`toggleKeystrokeCaption`**：显示/隐藏按键字幕面板。该面板显示正在输入的按键（包括快捷键）。非常适合演示！
- **`toggleVirtualKeyboard`**：显示/隐藏虚拟键盘

---

## 滚动

- **`scrollToStart`**：滚动到开头
- **`scrollToEnd`**：滚动到末尾
- **`scrollIntoView`**：滚动到可见区域

---

## 导航

- **`moveToNextChar`** / **`moveToPreviousChar`**：移动到下一个/上一个字符
- **`moveToNextPlaceholder`** / **`moveToPreviousPlaceholder`**：移动到下一个/上一个占位符
- **`moveToNextWord`** / **`moveToPreviousWord`**：移动到下一个/上一个单词
- **`moveToGroupStart`** / **`moveToGroupEnd`**：移动到组开头/结尾
- **`moveToMathfieldStart`** / **`moveToMathfieldEnd`**：移动到 mathfield 开头/结尾
- **`moveUp`** / **`moveDown`**：向上/向下移动
- **`moveToSuperscript`** / **`moveToSubscript`**：移动到上标/下标
- **`moveToOpposite`**：移动到对侧
- **`moveBeforeParent`** / **`moveAfterParent`**：移动到父元素之前/之后

---

## 扩展选区

- **`selectGroup`**：选择组
- **`selectAll`**：全选
- **`extendToNextChar`** / **`extendToPreviousChar`**：扩展到下一个/上一个字符
- **`extendToNextWord`** / **`extendToPreviousWord`**：扩展到下一个/上一个单词
- **`extendUp`** / **`extendDown`**：向上/向下扩展
- **`extendToNextBoundary`** / **`extendToPreviousBoundary`**：扩展到下一个/上一个边界
- **`extendToGroupStart`** / **`extendToGroupEnd`**：扩展到组开头/结尾
- **`extendToMathFieldStart`** / **`extendToMathFieldEnd`**：扩展到 mathfield 开头/结尾

---

## 数组

- **`addRowAfter`** / **`addRowBefore`**：在后/在前添加行
- **`addColumnAfter`** / **`addColumnBefore`**：在后/在前添加列
- **`removeRow`** / **`removeColumn`**：移除行/移除列

---

## 语音

- **`speak`**：此选择器接受一个可选参数，即决定朗读内容的字符串。有效值包括：
  - `all` — 全部
  - `left` — 左侧
  - `right` — 右侧
  - `selection` — 选区
  - `parent` — 父元素
  - `group` — 组

第二个参数决定朗读的内容是否应高亮显示。它是一个对象：`{withHighlighting: boolean}`（默认为 false）。注意：高亮功能目前仅在使用 Amazon AWS 语音合成器时有效。
