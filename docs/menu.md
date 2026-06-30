# 菜单

Mathfield 上下文菜单提供了一组命令，用于对 mathfield 执行常见操作。

**显示上下文菜单：**

- 在 mathfield 上**右键单击**
- **长按** mathfield
- 点击 mathfield 中的**菜单切换按钮**（汉堡图标）
- 在物理键盘上按 **ALT/OPTION+SPACE**、**FN+F10** 或 **MENU** 键

上下文菜单完全可访问。可以使用键盘导航，菜单项会被屏幕阅读器朗读。

要导航上下文菜单，请使用**方向键**。

也可以通过输入菜单项标签的部分字母来选择该项。

默认上下文菜单包含一组对大多数应用程序有用的命令，但您可以根据需要添加或删除命令来自定义菜单。

---

## 过滤菜单项

要选择显示哪些菜单项，请使用 `mf.menuItems` 属性的 `filter()` 方法。

例如，要省略所有与 Compute Engine 相关的命令（如 Evaluate、Simplify 和 Solve），可以按菜单项的 `id` 进行过滤：

```js
mf.menuItems = mf.menuItems.filter(item => !item.id?.startsWith('ce-'));
```

> **警告：** `mf.menuItems` 属性是**只读**属性。它返回原始菜单项数组的**副本**。
>
> 不要直接修改 `mf.menuItems` 元素的值。这将导致运行时错误。
>
> 例如，不要使用 `mf.menuItems[0].visible = false`。

---

## 替换菜单

要替换上下文菜单为自己的菜单，请设置 `mf.menuItems` 属性。

`menuItems` 属性是一个菜单项数组。

每个菜单项是一个具有以下属性的对象：

- **`type`**：可以是 `"command"`、`"divider"`、`"submenu"`、`"checkbox"`、`"radio"` 之一。默认为 `"command"`。
- **`label`**：菜单项显示的标签。可以是字符串字面量或返回字符串的函数。如果提供函数，则在菜单显示或键盘修饰键改变时会调用该函数更新标签。字符串的值被解释为 HTML 标记。
- **`ariaLabel`** 和 **`ariaDetails`**：如果提供，将用于设置菜单项的 `aria-label` 和 `aria-details` 属性，可供屏幕阅读器使用。与 `label` 属性一样，它们可以是字符串字面量或返回字符串的函数。
- **`visible`**、**`enabled`**、**`checked`**：状态标志，可设置为 `true` 或 `false`，用于控制菜单项的可见性、启用状态和选中状态。
- **`id`**：菜单项的唯一标识符。当菜单项被选中时，该值将传递给 `menu-select` 事件。
- **`data`**：与菜单项关联的任意数据负载（如果有）。
- **`submenu`**：如果类型为 `"submenu"`，则为选中该菜单项时显示的子菜单项数组。
- **`onMenuSelect`**：选中菜单项时调用的函数处理程序。

```js
mf.menuItems = [
  {
    label: 'Copy',
    onMenuSelect: () => console.log('Copy')
  },
  {
    label: 'Paste',
    onMenuSelect: () => console.log('Paste')
  },
  {
    type: 'divider'
  },
  {
    label: 'Submenu',
    submenu: [
      {
        label: 'Submenu 1',
        onMenuSelect: () => console.log('Submenu 1')
      },
      {
        label: 'Submenu 2',
        onMenuSelect: () => console.log('Submenu 2')
      }
    ]
  }
];
```

---

## 添加菜单项

要添加菜单项，请使用展开运算符（`...`）创建新的菜单项数组，并将新菜单项添加到原始数组的副本中：

```js
mf.menuItems = [
  {
    label: "Cancel",
    visible: () =>
      mf.isSelectionEditable && !mf.selectionIsCollapsed,
    onMenuSelect: () => mf.insert("\\cancel{#@}"),
  },
  ...mf.menuItems,
];
```

在此示例中，一个新的 Cancel 命令菜单项被添加到菜单的开头。

- `visible` 处理程序检查选区是否可编辑且未折叠。
- `onMenuSelect` 处理程序将选区替换为 `\cancel{}` 命令。`#@` 标记会被替换为当前选区，从而将选区包裹在 `\cancel{}` 命令中。

要在特定位置添加菜单项，请使用 `findIndex()` 方法查找要插入位置的菜单项索引：

```js
const isNonEmptySelection = () => mf.getValue(mf.selection).length > 0;

const getCancelArgument = () => {
  const selection = mf.getValue(mf.selection);
  // 检查选区是否为 \cancel{...} 命令
  const match = selection.match(/^\\cancel{([^}]*)}$/);
  return match ? match[1] : '';
};

const menuItems = mf.menuItems;

// 查找 "Cut" 菜单项的索引
const index = menuItems.findIndex(item => item.id === 'cut');

mf.menuItems = [
  // 在 "Cut" 菜单项之前添加新菜单项
  ...menuItems.slice(0, index),

  // 添加新命令
  { type: 'divider' },
  {
    label: "Cancel",
    visible: () =>
      mf.isSelectionEditable && isNonEmptySelection() && !getCancelArgument(),
    onMenuSelect: () =>
      mf.insert("\\cancel{#@}", { selectionMode: 'item' }),
  },
  {
    label: "Uncancel",
    visible: () => mf.isSelectionEditable && getCancelArgument(),
    onMenuSelect: () =>
      mf.insert(getCancelArgument(), { selectionMode: 'item' }),
  },
  { type: 'divider' },

  // 添加 "Cut" 菜单项之后的其余菜单项
  ...menuItems.slice(index)
];
```

在此示例中，新的菜单项被添加到 Cut 菜单项之后。我们通过将原始数组切片为两部分来创建新的菜单项数组：

- **第一部分**：Cut 项之前的菜单项。
- **第二部分**：Cut 项之后的菜单项。新菜单项添加在这两部分之间。

我们在新菜单项前后各添加了一个分隔线，这有助于将相关菜单项分组在一起。

我们添加了两个新菜单项：Cancel 和 Uncancel。Cancel 项仅在选区可编辑、非空且尚未是 cancel 命令时可见。Uncancel 项仅在选区可编辑且是 cancel 命令时可见。两个命令中最多只有一个会可见，从而允许用户对选区执行 cancel 或 uncancel 操作。

---

## 监听菜单事件

当菜单项被选中时，会调用其 `onMenuSelect` 处理程序，并触发一个 `menu-select` 自定义事件。

通常为每个菜单项提供一个 `onMenuSelect` 处理程序更简单，但您也可以监听 `menu-select` 事件，在单个事件处理程序中处理所有菜单项的选择。

`menu-select` 事件的 `detail` 属性包含以下属性：

- **`id`**：被选中的菜单项的 id。
- **`label`**：被选中的菜单项的标签。
- **`data`**：与菜单项关联的数据负载（如果有）。
- **`modifiers`**：包含选中菜单项时修饰键状态的对象。定义了以下属性：
  - `altKey`
  - `ctrlKey`
  - `metaKey`
  - `shiftKey`

上面使用 `onMenuSelect` 的示例可以重写为使用 `menu-select` 事件。注意，在这种情况下，菜单项具有 `id` 属性，用于标识被选中的菜单项。

```js
mf.menuItems = [
  {
    label: 'Copy',
    id: 'copy'
  },
  {
    label: 'Paste',
    id: 'paste'
  },
  {
    type: 'divider'
  },
  {
    label: 'Submenu',
    submenu: [
      {
        label: 'Submenu 1',
        id: 'submenu-1'
      },
      {
        label: 'Submenu 2',
        id: 'submenu-2'
      }
    ]
  }
];

mf.addEventListener('menu-select', (event) =>
  console.log('Menu item selected:', event.detail.id)
);
```

---

## 控制菜单可见性

要隐藏菜单切换按钮，请使用以下 CSS：

```css
math-field::part(menu-toggle) {
  display: none;
}
```

即使菜单切换按钮被隐藏，上下文菜单仍然可以通过键盘快捷键、右键单击或长按来访问。

要阻止菜单显示，请将 `mf.menuItems` 属性设置为空数组：

```js
mf.menuItems = [];
```

---

> MathField 0.110.0
>
> Compute Engine 0.66.0
