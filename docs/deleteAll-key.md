# 在虚拟键盘与编辑工具栏添加“清除（deleteAll）”按键

本文介绍两种在 MathLive 中创建“清除全部”（`deleteAll`）功能的方法：

- 在虚拟键盘布局中添加一个按键，按下时执行 `deleteAll`。
- 在页面/编辑工具栏添加一个按钮，点击时调用 mathfield 的 `deleteAll` 命令。

示例代码均为最小可用示例，按需集成到你的项目中。

---

## 1. 在虚拟键盘中添加 “清除” 按键

将按键添加到 `mathVirtualKeyboard.layouts` 的某行中：

```js
// 在脚本中设置布局（或在页面初始化时）
mathVirtualKeyboard.layouts = [
  {
    label: '示例',
    rows: [
      ['1','2','3',
        { label: '清除', class: 'action', command: ['performWithFeedback', 'deleteAll'], width: 1.5 }
      ]
    ]
  }
];
```

说明：
- `command` 可以直接使用 `['performWithFeedback','deleteAll']`（带反馈）或 `'deleteAll'`（直接调用）。
- 你也可以使用快捷键 `"[backspace]"`，其 Shift 变体通常为 `deleteAll`。

---

## 2. 在编辑工具栏添加“清除”按钮

如果页面有工具栏容器，将一个按钮插入该容器并在点击时调用 `mf.executeCommand('deleteAll')`：

```js
const mf = document.querySelector('math-field');
const toolbar = document.querySelector('#toolbar'); // 你的工具栏容器

const clearBtn = document.createElement('button');
clearBtn.type = 'button';
clearBtn.className = 'ml-toolbar-btn';
clearBtn.textContent = '清除';
clearBtn.addEventListener('click', () => mf.executeCommand('deleteAll'));

toolbar.appendChild(clearBtn);
```

说明：若想保留撤销/重做等现有按钮，请将新按钮追加到现有工具栏内。若需要视觉一致性，可使用项目的样式类（例如 `ml-toolbar-btn`）。

---

## 进阶提示

- 如需在按下后显示动画/反馈，请使用 `['performWithFeedback','deleteAll']`。某些项目中 `performWithFeedback` 会触发视觉提示。
- 若要将按键放到共享的 `fixedRows` 中（始终可见），可在布局的 `layers[].fixedRows` 中添加按键定义。

---

如果你希望我把示例直接写入某个 demo 页面或文档（例如 `examples/scrollable-keyboard/index.html`），我可以替你修改并提交补丁。 
