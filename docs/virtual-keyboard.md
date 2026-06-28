# 自定义虚拟键盘

**Mathfield 虚拟键盘** 是一个屏幕键盘，通过轻触即可输入数学专用符号。

本指南介绍如何自定义虚拟键盘。

[了解更多关于**使用数学虚拟键盘**](https://mathlive.io/mathfield/virtual-keyboard/)

---

## 控制虚拟键盘的显示时机

默认行为是：在支持触摸的设备（手机、平板、带触摸屏的笔记本电脑）上，当 mathfield 获得焦点时显示虚拟键盘。

可以通过 `mf.mathVirtualKeyboardPolicy` 属性或等价的 `math-virtual-keyboard-policy` 属性来改变此行为（设置其中一个即可，不要同时设置）。

| `mathVirtualKeyboardPolicy` | 说明 |
|---|---|
| `"auto"` | 在支持触摸的设备上，当 mathfield 获得焦点时显示虚拟键盘面板。此为默认行为。 |
| `"manual"` | 不自动显示虚拟键盘面板。可通过 `mathVirtualKeyboard.show()` 和 `mathVirtualKeyboard.hide()` 以编程方式控制虚拟键盘面板的显示与隐藏。 |
| `"sandboxed"` | 如果当前浏览上下文（iframe）有定义的容器或是顶级浏览上下文，则在其中显示虚拟键盘。 |

要在 mathfield 获得焦点时始终显示数学虚拟键盘（无论是否触摸设备），可使用：

```js
mf.mathVirtualKeyboardPolicy = "manual";

mf.addEventListener("focusin", () => mathVirtualKeyboard.show());

mf.addEventListener("focusout", () => mathVirtualKeyboard.hide());
```

---

## 控制虚拟键盘切换按钮的可见性

默认情况下，当 mathfield 可修改（即不是只读或禁用状态）时，会显示虚拟键盘切换按钮。

**要控制虚拟键盘切换按钮的可见性**，可使用 CSS。

例如，除非在触摸设备上，否则隐藏切换按钮：

```css
@media not (pointer: coarse) {
  math-field::part(virtual-keyboard-toggle) {
    display: none;
  }
}
```

---

## 自定义布局

虚拟键盘面板显示多个布局，可通过布局切换器切换：`numeric`（数字）、`symbols`（符号）、`alphabetic`（字母）和 `greek`（希腊字母）。

**要选择布局切换器中显示的布局**，可使用 `mathVirtualKeyboard.layouts` 属性。

例如，只显示 **numeric** 和 **symbols** 布局：

```js
document.querySelector('math-field').addEventListener('focus', () => {
  mathVirtualKeyboard.layouts = ["numeric", "symbols"];
  mathVirtualKeyboard.visible = true;
});
```

**要恢复为默认布局**，可使用：

```js
mathVirtualKeyboard.layouts = "default";
```

### 多个 Mathfield 使用不同布局

所有 mathfield 共享同一个虚拟键盘面板实例，布局也是共享的。

**要为特定 mathfield 显示不同的布局集**，在其获得焦点时更改 `mathVirtualKeyboardLayouts` 属性：

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

## 额外布局

除了 `numeric`、`symbols`、`alphabetic` 和 `greek`，还提供以下布局：

### 极简布局（Minimalist）

`"minimalist"` 布局专注于简单表达式的输入。

```js
mathVirtualKeyboard.layouts = ["minimalist"];
```

### 紧凑布局（Compact）

`"compact"` 布局类似于 `"minimalist"`，但按键包含变体。

```js
mathVirtualKeyboard.layouts = ["compact"];
```

### 纯数字布局（Numeric Only）

`"numeric-only"` 布局适用于纯数字输入。

```js
mathVirtualKeyboard.layouts = ["numeric-only"];
```

---

## 定义自定义布局

除了内置布局，您还可以定义自己的布局。

**定义自定义布局的最简单方法**是将 `mathVirtualKeyboard.layouts` 设置为一个对象字面量，其中包含 `rows` 属性（按键数组）。

为获得最佳效果，请确保每行不超过 10 个按键。

```js
document.querySelector('math-field').addEventListener('focus', () => {
  mathVirtualKeyboard.layouts = {
    rows: [
      [
        "+", "-", "\\times", "\\frac{#@}{#?}", "=", ".",
        "(", ")", "\\sqrt{#0}", "#@^{#?}",
      ],
      ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"],
    ]
  };
  mathVirtualKeyboard.visible = true;
});
```

每个按键是一个 LaTeX 字符串，既用作按键标签，也用作按下时插入的内容。

### 占位符标记

从上面的示例可以看出，定义按键的 LaTeX 片段可以包含一些特殊的占位符标记：

| 标记 | 说明 |
|---|---|
| `#@` | 替换为当前选区（如果有）。如果没有选区，则替换为光标左侧的隐式参数。例如，对于 `12+34`，如果光标在末尾，`#@` 将被替换为 `34`。 |
| `#0` | 如果有选区则替换为选区，否则替换为 `\placeholder{}` 命令 |
| `#?` | 替换为 `\placeholder{}` 命令 |

### 按键快捷方式

按键的值可以是 LaTeX 字符串，也可以是以下特殊值之一，对应标准按键。这些快捷方式定义了标签、外观、按下时的命令、Shift 命令和变体。

#### 导航与操作键
- `"[left]"`、`"[right]"`、`"[up]"`、`"[down]"` — 方向键
- `"[return]"` / `"[action]"` — 确认/换行
- `"[hide-keyboard]"` — 隐藏键盘
- `"[shift]"` — Shift 切换键
- `"[backspace]"` — 退格键（长按/Shift 为全部删除）
- `"[undo]"`、`"[redo]"` — 撤销/重做
- `"[cut]"`、`"[copy]"`、`"[paste]"` — 剪贴板操作

#### 符号键
- `"[.]"`、`"[,]"` — 小数点（根据 `decimalSeparator` 设置自动切换）
- `"[+]"`、`"[-]"`、`"[/]"`、`"[*]"`、`"[=]"` — 运算符
- `"[(]"`、`"[)]"` — 括号

#### 数字键
- `"[0]"` ~ `"[9]"` — 数字 0~9

#### 分隔符
- `"[separator]"` — 标准分隔符（默认宽度 1 倍）
- `"[separator-5]"` — 半宽分隔符（宽度 0.5 倍）
- `"[separator-10]"` — 标准宽度分隔符（宽度 1 倍）
- `"[separator-15]"` — 一倍半宽分隔符（宽度 1.5 倍）
- `"[separator-20]"` — 两倍宽分隔符（宽度 2.0 倍）
- `"[separator-50]"` — 五倍宽分隔符（宽度 5.0 倍）
- `"["hr"]"` — 水平分隔线（占一整行）

> **关于 `[separator]` 的详细说明：** 分隔符用于在键盘行中创建视觉间隔，将按键分组。它渲染为一个不可点击的空白区域。
> - `[separator]` 和 `[separator-10]` 宽度为 1 个标准按键宽度
> - `[separator-5]` 宽度为 0.5 个标准按键宽度，常用于在紧凑布局中分隔按键组
> - `[separator-15]`、`[separator-20]`、`[separator-50]` 用于创建更大的间隔
> - 分隔符也可以使用对象语法自定义：`{ class: 'separator', width: 0.5 }` 或 `{ class: 'separator w20' }`
> - `[hr]` 则渲染为一条贯穿整行的水平分割线，用于在视觉上区分不同的键盘区域

#### 颜色键
- `"[foreground-color]"` — 前景色选择器
- `"[background-color]"` — 背景色选择器

### 高级按键

要更精细地控制按键的外观和行为，可使用对象字面量，包含以下属性：

- **`label`**：按键的标签，使用系统字体显示。可包含 HTML 标记，例如 `"<span><i>x</i>&thinsp;²</span>"`。如果省略此属性，则使用 `latex` 属性作为按键标签。标签也可以是上述按键快捷方式之一（如 `[left]`）。如果使用了按键快捷方式，其他属性将覆盖快捷方式定义的值。

- **`latex`**：如果未提供 `label`，则 `latex` 属性的值用作按键标签。此属性还用于在按键按下时向 mathfield 插入内容。

- **`key`**：如果存在，按下按键时将模拟相应的物理键盘按键，可能触发键盘快捷键。

- **`insert`**：如果存在，按下按键时插入的 LaTeX 字符串。

- **`command`**：按下按键时要执行的命令。例如：`["performWithFeedback", "commit"]`。

- **`class`**：用于设置按键样式的 CSS 类。可以是自定义类（见下方的 `style` 层属性），也可以是以下一个或多个标准类：
  - `tex`：使用 TeX 字体显示标签。如果使用 `latex` 属性定义标签，则不需要 `tex` 类。
  - `ghost`：无边框或背景的按键
  - `small`：以较小尺寸显示标签
  - `action`："操作"按键（用于方向键、回车等）
  - `bottom`、`left`、`right`：标签的对齐方式
  - `hide-shift`：如果提供了 `shift` 属性，不显示按键右上角的 Shift 标签

- **`width`**：按键宽度，以标准按键的倍数表示。例如，0.5 表示半宽，1.5 表示一倍半宽。

- **`aside`**：按键下方显示的可选小标签。如果可用空间太小，可能不显示。

- **`shift`**：按下 Shift 键时按下此按键的 LaTeX 字符串或按键记录。

- **`variants`**：定义此按键变体的按键数组（字符串或按键记录）（见下文）。

如果既未提供 `insert` 也未提供 `command`，则使用 `latex` 或 `key` 属性来定义按下按键时插入的内容。

[了解更多关于**可用命令**](https://mathlive.io/mathfield/guides/commands/)

以下是一个基本键盘布局的示例：

```js
mathVirtualKeyboard.layouts = {
  label: 'Basic',
  rows: [
    [
      '[7]', '[8]', '[9]', '[+]',
      { label: '[separator]', width: 0.5 },
      { class: 'small', latex: '\\frac{#@}{#0}' },
      '\\varnothing', '\\infty', '\\in', '\\notin',
      '[separator]',
    ],
    [
      '[4]', '[5]', '[6]', '[-]',
      { label: '[separator]', width: 0.5 },
      '[(]', '[)]', '\\lt', '\\le', '\\hat{=}', '[separator]',
    ],
    [
      '[1]', '[2]', '[3]', '\\cdot',
      { label: '[separator]', width: 0.5 },
      '[', ']', '\\gt', '\\ge',
      { label: '[backspace]', width: 2 },
    ],
    [
      { label: '[0]', width: 2 }, '[.]', '\\colon',
      { label: '[separator]', width: 0.5 },
      '\\lbrace', '\\rbrace', '=', '\\ne', '[left]', '[right]',
    ],
  ],
};
```

### 按键变体

默认布局中的许多按键都包含**变体**。长按按键即可访问这些变体。变体通常是主按键的相关但较少使用的版本。

#### 默认变体机制（重要）

即使您没有显式定义 `variants` 属性，**按键快捷方式（如 `[0]`~`[9]`、`[+]`、`[=]`、`[(]` 等）自带默认变体**。这是因为每个按键快捷方式在 `KEYCAP_SHORTCUTS` 中预定义了 `variants` 属性，这些变体指向一个内置的变体注册表 `VARIANTS`。

例如，使用 `[0]` 时，其定义中包含 `variants: '0'`，而 `VARIANTS['0']` 预定义为 `['\\varnothing', '\\infty']`，因此长按数字 0 键会显示空集符号和无穷大符号。

以下是各按键快捷方式的**默认变体**一览：

| 按键快捷方式 | 默认变体内容 |
|---|---|
| `[0]` | `\varnothing`、`\infty` |
| `[1]` | `\frac{1}{#@}`、`#@^{-1}`、`\times 10^{#?}`、`\phi`、`\imaginaryI` |
| `[2]` | `\frac{1}{2}`、`#@^2`、`\sqrt2`、`\exponentialE` |
| `[3]` | `\frac{1}{3}`、`#@^3`、`\sqrt3`、`\pi` |
| `[4]` | `\frac{1}{4}`、`#@^4` |
| `[5]` | `\frac{1}{5}`、`#@^5`、`\sqrt5` |
| `[6]` | `\frac{1}{6}`、`#@^6` |
| `[7]` | `\frac{1}{7}`、`#@^7` |
| `[8]` | `\frac{1}{8}`、`#@^8` |
| `[9]` | `\frac{1}{9}`、`#@^9` |
| `[.]` | `.`、`,`、`;`、`\colon` |
| `[,]` | `{,}`、`.`、`;`、`\colon` |
| `[+]` | `\sum_{#0}^{#0}`、`\oplus` |
| `[-]` | `\pm`、`\ominus` |
| `[/]` | `/`、`\div`、`\%`、`\oslash` |
| `[*]` | `\prod_{#0}^{#0}`、`\otimes`、`\cdot` |
| `[=]` | `\neq`、`\equiv`、`\varpropto`、`\thickapprox`、`\lt`、`\gt`、`\le`、`\ge` |
| `[(]` | `\lbrack`、`\langle`、`\lfloor`、`\lceil`、`\lbrace` |
| `[)]` | `\rbrack`、`\rangle`、`\rfloor`、`\rceil`、`\rbrace` |
| `[backspace]` | Shift 为 `deleteAll`（全部删除） |

此外，字母按键（在字母布局中）也有默认变体。例如字母 `a` 的变体包括 `\aleph`、`\forall`、`å`、`à`、`á`、`â`、`ä`、`æ` 等。

#### 自定义变体

您可以通过在按键定义中指定 `variants` 属性来覆盖或添加变体。`variants` 属性的值是 `VirtualKeyboardKeycap` 数组。作为快捷方式，也可以使用字符串，相当于 `VirtualKeyboardKeycap` 的 `latex` 属性等于该字符串——即它将显示该 LaTeX 字符串作为按键标签，并在按下时插入该内容。

```js
rows: [
  [
    { latex: "a", variants: ["A", "\\alpha", "\\Alpha"] }
    ...
  ]
]
```

> **注意：** 如果您使用按键快捷方式（如 `[0]`）并希望保留其默认变体，不需要额外设置。如果您使用对象语法覆盖了按键定义，但想保留默认变体，可以显式设置 `variants` 属性，或使用 `...KEYCAP_SHORTCUTS['[0]']` 展开默认值。

### 层样式

如果要对某些按键应用自定义 CSS 类，可以使用 `style` 属性提供定义。注意，在这种情况下不能使用 `rows` 快捷方式，必须提供层的完整定义。

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
            '+', '-', '\\times', '\\frac{#@}{#?}', '=', '.',
            '(', ')', '\\sqrt{#0}', '#@^{#?}',
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
            { class: 'digit', latex: '0' },
          ],
        ],
      },
    ],
  },
  "alphabetic",
];
```

### 多层布局

大多数键盘布局只包含一个层。但如果您的布局包含多个层，可使用 `layers` 属性提供层数组。

```js
mathVirtualKeyboard.layouts = {
  layers: [
    {
      rows: [
        [
          "+", "-", "\\times", "\\frac{#@}{#?}", "=", ".",
          "(", ")", "\\sqrt{#0}", "#@^{#?}",
        ],
      ]
    },
    {
      rows: [
        ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"],
      ]
    }
  ],
};
```

### 多个布局

您也可以将默认布局与自定义布局混合使用。例如，在自定义布局后添加字母布局：

```js
mathVirtualKeyboard.layouts = [
  {
    label: "minimal",
    tooltip: "Only the essential",
    rows: [
      [
        "+", "-", "\\times", "\\frac{#@}{#?}", "=", ".",
        "(", ")", "\\sqrt{#0}", "#@^{#?}",
      ],
      ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"],
    ]
  },
  "alphabetic"
];
```

如果包含多个布局，建议提供 `label` 和 `tooltip`，以便在布局切换器中正确显示。

---

## 自定义虚拟键盘的外观

**要自定义虚拟键盘面板的外观**，在适用于虚拟键盘面板容器的选择器上设置以下 CSS 变量。默认容器是 `<body>` 元素：

```css
body {
  --keyboard-zindex: 3000;
}
```

也可以通过编程方式设置这些 CSS 变量：

```js
document.body.style.setProperty("--keyboard-zindex", "3000");
```

### 自定义虚拟键盘的层叠顺序

**要指定虚拟键盘相对于其他 DOM 元素的层叠顺序**，设置 `--keyboard-zindex` CSS 变量。

虚拟键盘的默认 `zindex` 为 `105`。

### 自定义虚拟键盘颜色

**要控制虚拟键盘文本和背景颜色的外观**，将以下 CSS 变量设置为 CSS 颜色值：

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

以下 CSS 变量是边框简写值：

- `--keyboard-border`
- `--keyboard-horizontal-rule`

### 自定义键盘大小

默认情况下，虚拟键盘的大小适合在触摸设备上舒适使用。其大小会根据容器（默认为视口）中的可用空间进行调整。
