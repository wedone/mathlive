# 自定义 Mathfield

Mathfield 的外观和行为具有高度可定制性。

本节将介绍一些自定义 mathfield 的方法。

---

## 样式设置

**要对 mathfield 进行样式设置**，定义针对 mathfield 的 CSS 规则，或使用 `<math-field>` 元素的 `style` 属性。

CSS 属性可以通过多种方式修改 mathfield 的外观，例如更改基础字号或添加边框。

**要移除 mathfield 的边框**，将 `border` 属性设置为 `none` 或 `0`。

**要更改 mathfield 的背景颜色**，使用 `background` 属性。

```html
<math-field style="border: none; background: #d8f0ff">
  x=\frac{-b\pm \sqrt{b^2-4ac}}{2a}
</math-field>
```

**要将 mathfield 显示为块级元素**（而非内联元素），添加 `style="display: block"` 属性。

```html
<p>答案：<math-field style="font-size:1.2rem">42</math-field>。</p>
<p>答案：<math-field style="font-size:2rem; display: block">3.1415</math-field></p>
```

### CSS 变量

**要自定义 mathfield 的外观**，在适用于 mathfield 元素的规则集中使用以下 CSS 变量（自定义属性）。

```css
math-field {
  --smart-fence-color: red;
}
```

虽然 CSS 样式对自定义组件"不可见"，但 CSS 变量会"穿透"并影响 `<math-field>` 自定义组件的内容。

在要自定义的 `<math-field>` 元素适用的任何选择器（例如 `body`）上设置这些 CSS 变量。

或者，也可以通过编程方式设置这些 CSS 变量：

```js
document.body.style.setProperty("--smart-fence-color", "red");
```

| CSS 变量 | 用途 |
|---|---|
| `--primary` | 主强调色，用于键盘切换按钮、菜单图标和虚拟键盘 |
| `--caret-color` | 插入点的颜色 |
| `--selection-color` | 选中内容的颜色 |
| `--selection-background-color` | 选中内容的背景颜色 |
| `--contains-highlight-background-color` | 包含光标的项目的背景颜色 |
| `--placeholder-color` | 占位符符号的颜色 |
| `--placeholder-opacity` | 占位符符号的不透明度（0-1） |
| `--smart-fence-color` | 智能分隔符的颜色（默认为 `current` 颜色） |
| `--smart-fence-opacity` | 智能分隔符的不透明度（默认为 `50%`） |
| `--highlight-text` | 指示光标在文本区域中的背景颜色 |
| `--text-font-family` | 文本区域中内容的字体栈 |
| `--latex-color` | LaTeX 区域中内容的颜色 |
| `--correct-color` | 提示在 `"correct"` 状态下的高亮颜色 |
| `--incorrect-color` | 提示在 `"incorrect"` 状态下的高亮颜色 |

对于颜色值，可以使用任何有效的 CSS 颜色值，如颜色名称，或使用 `transparent` 移除颜色。

**注意**：要更改占位符符号，请使用 `mf.placeholderSymbol` 属性。

```html
<style>
  math-field {
    --caret-color: red;
    --selection-background-color: lightgoldenrodyellow;
    --selection-color: darkblue;
  }
</style>
<math-field>x=\frac{-b\pm \sqrt{b^2-4ac}}{2a}</math-field>
```

您可以使用与虚拟键盘面板容器选择器关联的一些 CSS 变量来自定义虚拟键盘面板的外观和 zindex。

[了解更多关于自定义虚拟键盘外观](https://mathlive.io/mathfield/guides/virtual-keyboards/#custom-appearance)

### Mathfield 部件

由于 mathfield 是一个带有 Shadow DOM 的自定义元素，其内容不能直接被 Shadow DOM 外部的 CSS 规则访问。

但是，有一些部件可以使用 `::part()` 伪元素来设置 mathfield 内容的样式。

| 伪元素 | 用途 |
|---|---|
| `virtual-keyboard-toggle` | 虚拟键盘切换按钮 |
| `menu-toggle` | 菜单切换按钮 |
| `content` | 数学公式 |
| `container` | 包含公式、键盘切换按钮和菜单切换按钮的元素 |
| `keyboard-sink` | 捕获物理键盘输入的隐藏元素 |
| `placeholder` | 当 mathfield 为空时包含占位符属性的元素 |
| `prompt` | mathfield 内部的提示（`placeholder{}`） |

例如：

```css
/* 右对齐公式 */
math-field::part(content) {
  text-align: right;
}

/* 右对齐虚拟键盘切换按钮 */
math-field::part(container) {
  flex-flow: row-reverse;
}

/* 隐藏虚拟键盘切换按钮 */
math-field::part(virtual-keyboard-toggle) {
  display: none;
}

/* 隐藏菜单切换按钮 */
math-field::part(menu-toggle) {
  display: none;
}
```

**注意**：当菜单切换按钮被隐藏时，仍然可以通过右键单击 mathfield 打开菜单。您可以[自定义菜单](https://mathlive.io/mathfield/guides/menu/)来更改此行为。

### 占位符

**要自定义占位符文本**，在 `<math-field>` 元素上设置 `placeholder` 属性。

注意，`placeholder` 属性的内容被解释为 LaTeX 字符串。要将其显示为纯文本，请使用 `\text{}` 命令。

```html
<math-field placeholder="\text{输入公式}"></math-field>
```

### 焦点环

**要更改焦点环的外观**，使用 `:focus-within` 伪元素。

```html
<style>
  math-field:focus-within {
    outline: 4px solid #d7170b;
    border-radius: 4px;
    background: rgba(251, 187, 182, .1);
  }
</style>
<math-field>x=\frac{-b\pm \sqrt{b^2-4ac}}{2a}</math-field>
```

### 警告

**注意**：移除 CSS 中的 outline 会给使用键盘浏览网页的用户带来问题。不过，您可以更改 outline 的外观，例如指示错误状态。如果移除了 mathfield 的 outline，请确保用其他指示器替代，例如在封闭元素上显示 outline。

---

## 数学显示选项

公式的外观（无论是在可编辑的 mathfield 中还是作为静态表示）可以通过以下选项控制：

### 颜色

**要以编程方式更改公式的前景色（"墨水"）和背景色（"纸张"）**，使用 `mf.applyStyle()` 函数。

**要更改前景色**，使用 `\textcolor{}{}` 命令。**要更改背景色**，使用 `\colorbox{}{}` 命令。

这些命令的第一个参数是颜色，指定方式如下：
- 使用标准 CSS 格式的 RGB 颜色（`#d7170b`）
- [CSS 颜色名称](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value)（`goldenrod`）
- [dvips 颜色名称](https://ctan.org/pkg/colordvi)中的 68 种颜色之一（`cadetblue`）
- `ColorData[97, "ColorList"]` 中的 10 种 Mathematica 颜色（`m0` 到 `m9`）
- 使用 [`xcolor` 包](http://mirror.jmu.edu/pub/CTAN/macros/latex/contrib/xcolor/xcolor.pdf)语法定义的颜色，例如：`blue!20!black!30!green`

推荐使用以下颜色名称。它们可以通过虚拟键盘中的颜色键应用。

### 注意

这些颜色经过精心挑选，在色环上具有平衡的色相表示，具有相似的亮度和强度。它们将映射到与同名 `dvips` 颜色不同的颜色值。

### 注意

为了根据使用场景获得良好的可读性，这些颜色名称在用作前景色和背景色时将映射到不同的值。要使用特定的颜色值，请改用 RGB 颜色。

### 注意

**要自定义颜色名称的解释方式**，提供 `colorMap` 或 `backgroundColorMap` 函数。

### 大小

**要更改基础字号**，在 mathfield 或静态元素上将 `font-size` CSS 属性设置为所需值。

在公式中，可以通过 10 个值的字体比例指定大小，其中 1em 是 mathfield 或静态元素的基础字号。

| `fontSize` | 值 | LaTeX 命令 |
|---|---|---|
| 1 | 0.5 em | `\tiny` |
| 2 | 0.7 em | `\scriptsize` |
| 3 | 0.8 em | `\footnotesize` |
| 4 | 0.9 em | `\small` |
| 5 | 1 em | `\normalsize` 或 `\normal` |
| 6 | 1.2 em | `\large` |
| 7 | 1.44 em | `\Large` |
| 8 | 1.728 em | `\LARGE` |
| 9 | 2.074 em | `\huge` |
| 10 | 2.488 em | `\Huge` |

### 警告

在 TeX 中，大小命令在应用于数学时表现不一致。其他 TeX 实现也可能对大小命令有不同的解释。

### 数学布局

**要控制数学排版的某些方面**，使用命令 `\displaystyle`、`\textstyle`、`\scriptstyle`、`\scriptscriptstyle` 更改数学样式。

`displaystyle` 样式最适合公式周围有充足空间的情况。大型运算符（如 `\sum`）的上下限显示在运算符的上方和下方。分数的分子下方和分母上方以及关系运算符（`=`）和二元运算符（`+`）周围有充足的空间。

`textstyle` 样式在空间受限或公式周围有常规文本时很有用。大型运算符的上下限显示在运算符之后。分数的分子和分母使用较小的字号显示。但其他字符的字号不受影响。

`scriptstyle` 和 `scriptscriptstyle` 很少需要显式使用。内容使用较小的字号（分别为基础字号的 70% 和 50%）排列，运算符之间的间距最小化。但请注意，这些样式在某些情况下会自动使用。例如，使用 `displaystyle` 或 `textstyle` 时，大型运算符的上下限或符号的上标/下标会使用这些样式显示。注意，`displaystyle` 中的 `n=0` 在等号 `=` 周围没有空格，因为上下限以 `scriptstyle` 显示。

**要设置 mathfield 的默认数学样式**，设置 `mf.defaultMode` 属性或 `default-mode` 属性。

设置为 `"inline-math"` 使用 `textstyle`，设置为 `"math"` 使用 `displaystyle`。

```html
<p>答案是 <math-field default-mode="inline-math">x=\frac{-b\pm \sqrt{b^2-4ac}}{2a}</math-field>。</p>
```

默认情况下，mathfield 元素在内联上下文中（例如在 `<p>` 标签内）作为内联元素布局。

要使其作为块级元素布局，在 mathfield 上设置 `display: block`。

### 字母形状样式

**要控制哪些字母自动斜体化**，设置 `letterShapeStyle` 属性或 `letter-shape-style` 属性。

| `letterShapeStyle` | xyz | ABC | αβɣ | ΓΔΘ |
|---|---|---|---|---|
| `iso` | 斜体 | 斜体 | 斜体 | 斜体 |
| `tex` | 斜体 | 斜体 | 斜体 | 正体 |
| `french` | 斜体 | 正体 | 正体 | 正体 |
| `upright` | 正体 | 正体 | 正体 | 正体 |

在 [ISO](https://www.nist.gov/pml/special-publication-811) 样式中，小写和大写罗马字母以及小写和大写希腊字母在用作变量时被斜体化。数学常数如 e 则使用正体。

TeX 传统上实现了一种布局选项，将罗马字母和小写希腊字母斜体化，但不包括大写希腊字母。

法国排版惯例是只将小写罗马字母斜体化。

默认的字母形状样式是 `auto`：如果系统区域设置为"french"，则使用 `french` 样式，否则使用 `tex`。

---

## 编辑选项

可以通过在 mathfield 上设置某些属性（或 `<math-field>` 标签上的等价属性）来自定义 mathfield 的编辑行为。

- **`defaultMode`**：mathfield 的默认模式。可以是以下之一：
  - `"inline-math"`：使用内联数学模式
  - `"math"`：使用显示数学模式
  - `"text"`：使用文本模式

- **`removeExtraneousParentheses`**：自动移除分子或分母周围多余的括号

- **`scriptDepth`**：下标或上标的最大层数。设置为 0 可阻止输入上标和下标

- **`smartFence`**：自动将括号转换为 `\left...\right` 标记

- **`smartMode`**：检测到文本输入时切换到文本模式，例如输入"if x > 0"时

- **`smartSuperscript`**：输入数字时自动退出上标模式

这些属性也可以作为参数传递给 `new MathfieldElement()`，在以编程方式创建 mathfield 元素时使用。

### 处理空格键

在传统数学排版中，空格没有效果：公式中元素的间距由元素的性质决定：数字、标点、关系运算符、二元或一元运算符等。

**要控制公式中的间距**，使用一些 LaTeX 间距命令：`\quad`、`\qquad`、`\!`、`\,`（薄空格）、`\:`（中等空格）、`\;`（厚空格）、`\enskip` 或 `\enspace`。

默认情况下，在数学模式下按空格键不会插入任何内容。

**要在按下空格键时插入 LaTeX 命令**，将 `MathfieldElement.mathModeSpace` 属性的值设置为该命令：

```js
MathfieldElement.mathModeSpace = '\\:';
```

### 关闭 LaTeX 模式

按下 `\`（反斜杠）或 ESC 键会切换到 LaTeX 模式，在此模式下可以输入原始 LaTeX 命令。对于熟悉 LaTeX 的用户来说，这是输入或编辑表达式中 LaTeX 的强大方式。但是，不熟悉 LaTeX 的用户如果意外按下这些键可能会感到困惑。

**要阻止 LaTeX 模式被启用**，拦截触发键并调用 `preventDefault()`。

```js
mf.addEventListener(
  'keydown',
  (ev) => {
    if (ev.key === '\\') {
      ev.preventDefault();
      mf.executeCommand(['insert', '\\backslash']);
    } else if (ev.key === 'Escape') {
      ev.preventDefault();
    }
  },
  { capture: true }
);
```

---

## 本地化

Mathfield 的用户界面支持英语、阿拉伯语、德语、希腊语、西班牙语、波斯语、法语、意大利语、日语、波兰语和俄语。

使用的语言会自动检测，但可以通过 `MathfieldElement.locale` 静态属性覆盖。设置此属性将影响页面上的所有 mathfield 元素。

```html
<math-field id=formula>x=\frac{-b\pm \sqrt{b^2-4ac}}{2a}</math-field>
```

```js
await customElements.whenDefined('math-field');
const locale = MathfieldElement.locale;
console.log("区域设置:", locale);
console.log(MathfieldElement.strings[locale.substring(0, 2)]);
```

### 小数标记

世界上大约有一半的国家使用点 `.` 或逗号 `,` 作为小数标记。

**要更改十进制数字使用的小数标记**，将 `MathfieldElement.decimalSeparator` 属性设置为 `","` 或 `"."`。

当设置为 `","` 时，在物理键盘上按 `,` 键将插入 `{,}` LaTeX 字符串（如果在数学模式下且位于数字之前）。

LaTeX 序列 `{,}` 传统上用于正确排版逗号并确保其周围有合适的间距。没有 `{}` 时，`,` 被解释为分隔符，周围会有过大的间距。

当设置为 `","` 时，虚拟键盘也会相应更改，使 `.` 按键标签变为 `,`，并在适当时上下文地插入 `{,}`。

```js
MathfieldElement.decimalSeparator = ",";
```

### 分数导航顺序

使用键盘上的方向键导航分数时...
