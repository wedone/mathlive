# 自定义虚拟键盘

Mathfield 虚拟键盘是一个显示在屏幕上的键盘，允许用户通过轻点访问数学输入的专用符号。

本指南解释如何自定义虚拟键盘。

[了解如何使用数学虚拟键盘](https://mathlive.io/mathfield/virtual-keyboard/)

---

## 控制何时显示虚拟键盘

默认行为是在触摸设备上（手机、平板和带触控屏的笔记本电脑）当 mathfield 获得焦点时显示虚拟键盘。

可以通过 `mf.mathVirtualKeyboardPolicy` 属性或等价的 `math-virtual-keyboard-policy` 属性更改此行为（请只设置其一，而不是同时设置两者）。

| 值            | 说明                                                                                                                   |
| ------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `"auto"`      | 在触摸设备上，当 mathfield 获得焦点时显示虚拟键盘面板。这是默认行为。                                                  |
| `"manual"`    | 不自动显示虚拟键盘面板。可以使用 `mathVirtualKeyboard.show()` 和 `mathVirtualKeyboard.hide()` 在代码中控制面板可见性。 |
| `"sandboxed"` | 如果当前浏览上下文（iframe）有定义的容器或是顶层浏览上下文，则在当前浏览上下文中显示虚拟键盘。                         |

要在触摸设备和非触摸设备上，只要 mathfield 获得焦点就显示数学虚拟键盘，请使用：

```js
mf.mathVirtualKeyboardPolicy = "manual";
mf.addEventListener("focusin", () => mathVirtualKeyboard.show());
mf.addEventListener("focusout", () => mathVirtualKeyboard.hide());
```

---

## 控制虚拟切换按钮的可见性

当 mathfield 可以修改时（即不是只读或禁用状态），默认会显示虚拟键盘切换按钮。

要控制该按钮的可见性，请使用 CSS。

例如，要在非触摸设备上隐藏切换按钮，可以使用：

```css
@media not (pointer: coarse) {
  math-field::part(virtual-keyboard-toggle) {
    display: none;
  }
}
```

---

## 自定义布局

虚拟键盘面板显示多个布局，可以通过布局切换器切换：`numeric`、`symbols`、`alphabetic` 和 `greek`。

要选择哪些布局出现在布局切换器中，请使用 `mathVirtualKeyboard.layouts` 属性。

例如，要仅显示 numeric 和 symbols 布局：

```js
mathVirtualKeyboard.layouts = ["numeric", "symbols"];
mathVirtualKeyboard.visible = true;
```

要恢复默认布局，请使用：

```js
mathVirtualKeyboard.layouts = "default";
```

### 为多个 mathfield 使用不同布局

虚拟键盘面板和布局是所有 mathfield 共享的。如果希望特定 mathfield 显示不同布局，请在该 mathfield 获得焦点时更改其 `mathVirtualKeyboardLayouts` 属性。

```js
// mathfield mf1 的布局
mf1.addEventListener("focusin", () => {
  mathVirtualKeyboard.layouts = ["numeric", "symbols"];
});

// mathfield mf2 的布局
mf2.addEventListener("focusin", () => {
  mathVirtualKeyboard.layouts = ["minimalist"];
});
```

---

## 附加布局

除 `numeric`、`symbols`、`alphabetic` 和 `greek` 外，还提供以下布局：

### Minimalist 布局

`"minimalist"` 布局专注于输入简单表达式。

```js
mathVirtualKeyboard.layouts = ["minimalist"];
```

### Compact 布局

`"compact"` 布局与 `"minimalist"` 相似，但按键包含变体。

```js
mathVirtualKeyboard.layouts = ["compact"];
```

### Numeric Only 布局

`"numeric-only"` 布局适合纯数字输入。

```js
mathVirtualKeyboard.layouts = ["numeric-only"];
```

---

## 定义自定义布局

除了内置布局，你也可以定义自己的布局。

定义自定义布局的最简单方式是将 `mathVirtualKeyboard.layouts` 设置为具有 `rows` 属性的对象字面量，`rows` 是一个按键阵列。

为了获得最佳效果，建议每行不要超过 10 个按键。

```js
mathVirtualKeyboard.layouts = {
  rows: [
    [
      "+", "-", "\\times", "\\frac{#@}{#?}", "=", ".",
      "(", ")", "\\sqrt{#0}", "#@^{#?}"
    ],
    ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"],
  ]
};
mathVirtualKeyboard.visible = true;
```

每个按键通常是一个 LaTeX 字符串，该字符串既用作按键标签，也用作按下时插入的内容。

### 占位符令牌

LaTeX 片段中可以包含一些特殊占位符令牌：

| 令牌 | 含义                                                                                                                               |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `#@` | 如果有选区，则替换为选区；如果没有选区，则替换为插入点左侧的隐式参数。例如，对于 `12+34`，如果插入点在末尾，`#@` 会被替换为 `34`。 |
| `#0` | 如果有选区，则替换为选区，否则替换为 `\placeholder{}` 命令。                                                                       |
| `#?` | 替换为 `\placeholder{}` 命令。                                                                                                     |

### 按键快捷方式

按键的值可以是 LaTeX 字符串，也可以是下列特殊值之一，对应标准按键。这些快捷方式定义了按键标签、外观、按下时执行的命令、按住 Shift 时的命令以及按键变体。

`"[left]"` `"[right]"` `"[return]"` `"[hide-keyboard]"` `"[shift]"`

`"[backspace]"` `"[undo]"` `"[redo]"` `"[cut]"` `"[copy]"` `"[paste]"`

`"[.]"` `"[+]"` `"[-]"` `"[/]"` `"[*]"` `"[=]"`

`"[(]"` `"[)]"`

`"[0]"` `"[1]"` `"[2]"` `"[3]"` `"[4]"` `"[5]"` `"[6]"` `"[7]"` `"[8]"` `"[9]"`

`"[separator]"` `"[hr]"`

`"[foreground-color]"` `"[background-color]"`

### 高级按键

要更精细地控制按键的外观和行为，可以使用对象字面量定义按键：

- `label`：按键显示的标签，使用系统字体。可以包含 HTML 标记，例如 `"<span><i>x</i>&thinsp;²</span>"`。如果未提供该属性，则使用 `latex` 属性作为标签。
- `latex`：如果未提供 `label`，则该属性值用于按键标签。该属性还用于按下按键时插入 mathfield 的内容。
- `key`：如果存在，按下按键时会模拟对应的物理键，可能触发键盘快捷键。
- `insert`：如果存在，按下按键时插入的 LaTeX 字符串。
- `command`：按下按键时执行的命令，例如 `['performWithFeedback', 'commit']`。
- `class`：用于样式按键的 CSS 类集合。可以是自定义类（见下面的 `style` 层属性），也可以是以下标准类之一：
  - `tex`：使用 TeX 字体渲染标签。如果使用 `latex` 属性定义标签，则无需使用该类。
  - `ghost`：无边框、无背景的按键。
  - `small`：以较小字体显示标签。
  - `action`：表示箭头、回车等“动作”按键。
  - `bottom`、`left`、`right`：标签对齐方式。
  - `hide-shift`：如果提供了 `shift` 属性，则不显示按键右上角的 Shift 标签。
- `width`：按键宽度，单位为标准按键宽度的倍数。例如 0.5 表示半宽按键，1.5 表示一倍半宽按键。
- `aside`：按键下方显示的可选小标签。如果空间不足，该标签可能不会显示。
- `shift`：在按住 Shift 键时按下该按键的 LaTeX 字符串或按键记录。
- `variants`：定义该按键的变体的按键数组。可以是字符串或按键记录。

如果既没有提供 `insert`，也没有提供 `command`，则会使用 `latex` 或 `key` 属性来确定按下按键时插入的内容。

### 按键变体

默认布局中许多按键都提供变体，用户可通过长按主键打开变体面板并选择某一变体。下面说明 `variants` 支持的格式、含义与限制，并给出示例。

- 支持的格式：
  - 字符串（例如 `"a"` 或 `'\\alpha'`）：被视为快捷写法，相当于 `{ latex: "..." }`，按下时插入对应的 LaTeX。
  - 对象（`Partial<VirtualKeyboardKeycap>`）：完整按键定义，可包含 `label`、`latex`、`insert`、`command`、`class`、`width`、`aside`、`shift`、`variants`、`layer`、`stickyVariantPanel` 等字段，渲染时这些属性都会生效（例如可为某个变体指定 `class` 来改变样式）。
  - 预定义集合 ID（例如 `'0'`, `'.'`, `'foreground-color'`）：会从内部的 `VARIANTS` 表中展开为一组字符串/对象。

- 方括号语法（例如 `"[left]"`、`"[shift]"`、`"[0]"`）不是数组而是“快捷键名”，由代码中的 `KEYCAP_SHORTCUTS` 展开为预定义按键对象（通常包含 `command`、`class` 等）。

- 嵌套与限制：
  - 数据结构上，你可以在某个变体对象中再设置 `variants` 字段（normalize 会保留它）。
  - 但 UI 行为上，变体面板不会在面板内再弹出“二级变体”面板：长按主键打开一级变体面板后，点击某个变体会直接执行该变体的动作或插入内容，面板内项不支持再次长按打开子面板。

- 使用示例：

```js
// 简单字符串和对象混合
rows: [
  [
    {
      latex: 'a',
      variants: ['A', '\\alpha', '\\Alpha']
    },
    {
      latex: '\\sum',
      variants: [
        { latex: '\\sum_{#0}^{#0}', class: 'small' },
        '\\oplus'
      ]
    }
  ]
]

// 使用预定义集合 ID
{ label: '0..9', variants: '0' }

// 使用快捷键名展开为带命令的按键
['[left]', '[right]', '[shift]']
```

总结：你可以为变体使用对象并在其中指定 `class` 等高级属性来控制样式或命令；但不要依赖在变体面板中继续弹出子变体（UI 不支持二级弹出）。

### 层样式

如果要为某些按键应用自定义 CSS 类，可以使用 `style` 属性提供定义。注意，在这种情况下不能使用 `rows` 快捷方式，必须提供完整的层定义。

```js
mathVirtualKeyboard.layouts = [
  {
    label: "minimal",
    tooltip: "Only the essential",
    layers: [
      {
        style: ".digit { background: blue; color: white }",
        rows: [
          [
            '+', '-', '\\times', '\\frac{#@}{#?}', '=', '.', '(', ')', '\\sqrt{#0}', '#@^{#?}'
          ],
          [
            { class: 'digit', latex: '1' },
            { class: 'digit', latex: '2' },
            { class: 'digit', latex: '3' },
            { class: 'digit', latex: '4' },
            { class: 'digit', latex: '5' },
            { class: 'digit', latex: '6' },
            { class: 'digit', latex: '7' },
            { class: 'digit', latex: '8' },
            { class: 'digit', latex: '9' },
            { class: 'digit', latex: '0' }
          ]
        ]
      }
    ]
  },
  "alphabetic"
];
```

### 多层

大多数键盘布局由单个层组成。但如果布局包含多个层，请使用 `layers` 属性提供一个层数组。

在这种情况下，键盘会包含多个“页面”或“层”，每次只显示一个层的按键。注意：MathLive 不会为同一个布局自动绘制“层切换”选项卡。

要在层之间切换，需要在布局中提供一个按键，并为该按键设置 `command: 'switchKeyboardLayer("<layer-id>")'`（或等价的数组形式）。例如，第一层可以显示操作符，第二层显示数字键，按下自定义的“切换层”按键即可在两者之间切换。

```js
mathVirtualKeyboard.layouts = {
  layers: [
    {
      id: 'symbols',
      rows: [
        [
          {
            label: '123',
            command: 'switchKeyboardLayer("numbers")'
          },
          '+', '-', '\\times', '\\frac{#@}{#?}', '=', '.', '(', ')', '\\sqrt{#0}', '#@^{#?}'
        ]
      ]
    },
    {
      id: 'numbers',
      rows: [
        [
          {
            label: '+-*/',
            command: 'switchKeyboardLayer("symbols")'
          },
          '1', '2', '3', '4', '5', '6', '7', '8', '9', '0'
        ]
      ]
    }
  ]
};
```

> 上述示例并不是同时显示两个层，而是定义了一个拥有两个可切换层的布局：一个层包含符号按键，另一个层包含数字按键。用户通过按键命令切换当前层。

### 多布局

你也可以将默认布局与自定义布局混合使用。例如，在自定义布局之后添加 `alphabetic`：

```js
mathVirtualKeyboard.layouts = [
  {
    label: "minimal",
    tooltip: "Only the essential",
    rows: [
      [
        "+", "-", "\\times", "\\frac{#@}{#?}", "=", ".",
        "(", ")", "\\sqrt{#0}", "#@^{#?}"
      ],
      ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"]
    ]
  },
  "alphabetic"
];
```

如果包含多个布局，最好为它们提供 `label` 和 `tooltip`，以便在布局切换器中正确显示。

---

## 自定义虚拟键盘外观

要自定义虚拟键盘面板的外观，请在适用于虚拟键盘面板容器的选择器上设置 CSS 变量，默认容器为 `<body>`。

### 自定义堆叠顺序

要指定虚拟键盘相对于其他 DOM 元素的堆叠顺序，请设置 `--keyboard-zindex` CSS 变量。

虚拟键盘的默认 z-index 是 `105`。

```css
body {
  --keyboard-zindex: 3000;
}
```

你也可以通过 JavaScript 设置这些 CSS 变量：

```js
document.body.style.setProperty("--keyboard-zindex", "3000");
```

### 自定义颜色

要控制虚拟键盘文本和背景颜色，请将以下 CSS 变量设置为 CSS 颜色值：

- `--keyboard-accent-color`
- `--keyboard-toolbar-text`
- `--keyboard-toolbar-text-active`
- `--keyboard-toolbar-background`
- `--keyboard-toolbar-background-hover`
- `--keyboard-toolbar-background-selected`
- `--keycap-background`
- `--keycap-background-hover`
- `--keycap-background-active`
- `--keycap-background-pressed`
- `--keycap-border`
- `--keycap-border-bottom`
- `--keycap-text`
- `--keycap-text-active`
- `--keycap-text-hover`
- `--keycap-text-pressed`
- `--keycap-shift-text`
- `--keycap-shift-color`
- `--keycap-primary-background`
- `--keycap-primary-text`
- `--keycap-primary-background-hover`
- `--keycap-secondary-background`
- `--keycap-secondary-background-hover`
- `--keycap-secondary-text`
- `--keycap-secondary-border`
- `--keycap-secondary-border-bottom`
- `--box-placeholder-color`
- `--variant-panel-background`
- `--variant-keycap-text`
- `--variant-keycap-text-active`
- `--variant-keycap-background-active`
- `--keyboard-border`
- `--keyboard-horizontal-rule`

### 自定义键盘大小

默认情况下，虚拟键盘的大小适合触摸设备使用。它会根据容器中的可用空间调整大小，默认容器是视口。

如果希望键盘更紧凑，以便给内容留出更多空间，可以使用一些 CSS 变量来控制其外观。将这些变量设置在适用于整个文档的规则中，例如 `body`：

```css
body {
  --keycap-height: 24px;
  --keycap-font-size: 16px;
  --keycap-shift-font-size: 9px;
  --keycap-small-font-size: 9px;
  --keycap-extra-small-font-size: 9px;
  --keyboard-toolbar-font-size: 16px;
  --keycap-gap: 1px;
}
```

以下 CSS 变量可用于调整布局：

- `--keycap-height`
- `--keycap-max-width`
- `--keycap-gap`
- `--keycap-font-size`
- `--keycap-shift-font-size`
- `--keycap-small-font-size`
- `--keycap-extra-small-font-size`
- `--keycap-secondary-border-bottom`
- `--keyboard-toolbar-font-size`
- `--keyboard-padding-horizontal`
- `--keyboard-padding-top`
- `--keyboard-padding-bottom`
- `--keyboard-row-padding-left`
- `--keyboard-row-padding-right`
- `--variant-keycap-length`
- `--variant-keycap-font-size`
- `--variant-keycap-aside-font-size`

---

## 阻止物理键盘输入

如果希望仅使用虚拟键盘输入并忽略物理键盘按键，请在捕获阶段监听 `keydown` 事件并调用 `preventDefault()`，同时在 mathfield 获得焦点时显示虚拟键盘。

```js
mf.addEventListener("keydown", (evt) => evt.preventDefault(), { capture: true });
mf.addEventListener("focus", () => mathVirtualKeyboard.show());
```

---

## 在自定义容器中显示虚拟键盘

默认情况下，虚拟键盘插入到文档 `body` 元素的末尾。

某些情况下，您可能希望将虚拟键盘显示在其他容器中。

例如，当使用包含 mathfield 的[全屏元素](https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API)时，您可能希望将虚拟键盘面板附加到全屏元素上，以确保它可见。

要选择虚拟键盘附加到哪个 DOM 元素，请将 `mathVirtualKeyboard.container` 属性设置为所需的 DOM 元素。

> **警告：** 容器元素的宽度应至少为 320px，以确保默认布局能够适配。容器元素的高度将根据虚拟键盘调整。

---

## 响应虚拟键盘几何变化

虚拟键盘面板使用 `position: absolute` 相对于容器元素定位，因此不会影响容器元素的布局。

不过，容器元素可能需要调整其布局以给虚拟键盘面板留出空间。例如，如果容器元素是全屏元素，则可能需要调整其高度。

要响应虚拟键盘几何变化，请监听 `mathVirtualKeyboard` 对象的 `geometrychange` 事件。

虚拟键盘的边界矩形可通过 `mathVirtualKeyboard.boundingRect` 属性获取。

例如，为使容器元素为虚拟键盘面板预留高度：

```js
mathVirtualKeyboard.addEventListener("geometrychange", () => {
  container.style.height = mathVirtualKeyboard.boundingRect.height + "px";
});
```

---

## 自定义字母布局

默认情况下，`"alphabetic"` 布局根据区域设置确定（英语国家使用 QWERTY，法语国家使用 AZERTY 等）。

要选择不同的字母键盘布局，例如 DVORAK 或 COLEMAK，请使用 `mathVirtualKeyboard.alphabeticLayout` 属性。

```js
const mf = document.querySelector('math-field');
mf.addEventListener('focus', () => {
  mathVirtualKeyboard.layouts = ["alphabetic"];
  mathVirtualKeyboard.alphabeticLayout = "dvorak";
  mathVirtualKeyboard.visible = true;
});
```
