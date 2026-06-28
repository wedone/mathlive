# Mathfield API 参考

## Mathfield

### MathfieldElement

`MathfieldElement` 类是一个提供数学输入字段的 DOM 元素。

它是标准 [`HTMLElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement) 类的子类，因此继承了其所有属性和方法，例如 `style`、`tabIndex`、`addEventListener()`、`getAttribute()` 等。

`MathfieldElement` 类提供了额外的属性和方法来控制 `<math-field>` 元素的显示和行为。

**要实例化 `MathfieldElement`**，在 HTML 中使用 `<math-field>` 标签。也可以使用 `new MathfieldElement()` 以编程方式实例化。

```js
// 1. 创建新的 MathfieldElement
const mf = new MathfieldElement();

// 2. 将其附加到 DOM
document.body.appendChild(mf);

// 3. 在 mathfield 附加到 DOM 后修改选项
mf.addEventListener("mount", () => {
  mf.smartFence = true;
});
```

在[自定义 Mathfield](https://mathlive.io/mathfield/guides/customizing/) 指南中了解更多关于自定义 mathfield 外观和行为的信息。

#### MathfieldElement CSS 变量

**要自定义 mathfield 的外观**，在适用于 mathfield 的规则集中声明以下 CSS 变量（自定义属性）。

```css
math-field {
  --hue: 10; /* 将高亮颜色和光标设置为偏红色调 */
}
```

或者，也可以通过编程方式设置这些 CSS 变量：

```js
document.body.style.setProperty("--hue", "10");
```

了解更多关于可用于自定义的 [CSS 变量](https://mathlive.io/mathfield/api/#css-variables)。

您可以使用与虚拟键盘面板容器选择器关联的一些 CSS 变量来自定义虚拟键盘面板的外观和 zindex。

了解更多关于[自定义虚拟键盘外观](https://mathlive.io/mathfield/api/#custom-appearance)。

#### MathfieldElement CSS 部件

除了 CSS 变量，mathfield 还暴露了[可用于设置 mathfield 样式的 CSS 部件](https://mathlive.io/mathfield/api/#mathfield-parts)。

例如，隐藏菜单按钮：

```css
math-field::part(menu-toggle) {
  display: none;
}
```

#### MathfieldElement 属性

属性是作为 `<math-field>` 标签一部分设置的键值对：

```html
<math-field letter-shape-style="tex"></math-field>
```

支持的属性如下表所示，它们对应可以在 `MathfieldElement` 对象上直接更改的属性：

```js
mf.value = "\\sin x";
mf.letterShapeStyle = "tex";
```

属性和属性的值是相互映射的，这意味着您可以更改其中一个，例如：

```js
mf.setAttribute("letter-shape-style", "french");
console.log(mf.letterShapeStyle);
// 结果: "french"

mf.letterShapeStyle = "tex";
console.log(mf.getAttribute("letter-shape-style"));
// 结果: "tex"
```

一个例外是 `value` 属性，它不会映射到 `value` 属性。为了与其他 DOM 元素保持一致，`value` 属性保持其初始值。

| 属性 | 属性（Property） |
|---|---|
| `disabled` | `mf.disabled` |
| `default-mode` | `mf.defaultMode` |
| `letter-shape-style` | `mf.letterShapeStyle` |
| `min-font-scale` | `mf.minFontScale` |
| `max-matrix-cols` | `mf.maxMatrixCols` |
| `popover-policy` | `mf.popoverPolicy` |
| `math-mode-space` | `mf.mathModeSpace` |
| `read-only` | `mf.readOnly` |
| `remove-extraneous-parentheses` | `mf.removeExtraneousParentheses` |
| `smart-fence` | `mf.smartFence` |
| `smart-mode` | `mf.smartMode` |
| `smart-superscript` | `mf.smartSuperscript` |
| `inline-shortcut-timeout` | `mf.inlineShortcutTimeout` |
| `script-depth` | `mf.scriptDepth` |
| `value` | `value` |
| `math-virtual-keyboard-policy` | `mathVirtualKeyboardPolicy` |

此外，还支持以下 DOM 元素[全局属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes)：

- `class`
- `data-*`
- `hidden`
- `id`
- `item*`
- `style`
- `tabindex`

#### MathfieldElement 事件

**要监听这些事件**，使用 `mf.addEventListener()`。对于带有额外参数的事件，参数可在 `event.detail` 中获取。

| 事件名称 | 描述 |
|---|---|
| `beforeinput` | mathfield 的值即将被修改。 |
| `input` | mathfield 的值已被修改。几乎每次在 mathfield 中按键时都会触发。`evt.data` 属性包含 `evt.inputType` 的副本。参见 `InputEvent`。 |
| `change` | 用户已提交 mathfield 的值。当用户按下 **回车** 或离开 mathfield 时触发。 |
| `selection-change` | mathfield 中的选区（或光标位置）已更改 |
| `mode-change` | mathfield 的模式（`math`、`text`）已更改 |
| `undo-state-change` | 撤销栈的状态已更改。`evt.detail.type` 指示是拍摄了快照还是执行了撤销操作。 |
| `read-aloud-status-change` | 朗读操作的状态已更改 |
| `before-virtual-keyboard-toggle` | 虚拟键盘面板的可见性即将更改。`evt.detail.visible` 属性指示键盘是否将可见。在 `window.mathVirtualKeyboard` 上监听此事件。 |
| `virtual-keyboard-toggle` | 虚拟键盘面板的可见性已更改。在 `window.mathVirtualKeyboard` 上监听此事件。 |
| `geometrychange` | 虚拟键盘的几何形状已更改。`evt.detail.boundingRect` 属性是虚拟键盘的新边界矩形。在 `window.mathVirtualKeyboard` 上监听此事件。 |
| `blur` | mathfield 正在失去焦点 |
| `focus` | mathfield 正在获得焦点 |
| `move-out` | 用户按下了**方向**键或 **Tab** 键，但无处可去。这是根据需要将焦点更改到另一个元素的机会。`detail: {direction: 'forward' \| 'backward' \| 'upward' \| 'downward'}` |
| `keypress` | 用户按下了物理键盘键 |
| `mount` | 元素已附加到 DOM |
| `unmount` | 元素即将从 DOM 中移除 |

#### 继承

- `HTMLElement`

#### new MathfieldElement()

```js
new MathfieldElement(options?): MathfieldElement
```

要以编程方式创建新的 mathfield，使用：

```js
let mfe = new MathfieldElement();

// 设置初始值和选项
mfe.value = "\\frac{\\sin(x)}{\\cos(x)}";

// 选项可以设置为属性（对于简单选项）...
mfe.setAttribute("letter-shape-style", "french");

// ...或使用属性
mfe.letterShapeStyle = "french";

// 将元素附加到 DOM
document.body.appendChild(mfe);
```

##### options?

`Partial<MathfieldOptions>`

---

### 访问和更改内容

##### MathfieldElement.errors

返回 LaTeX 语法错误数组（如果有）。

##### MathfieldElement.expression

```js
get expression(): any
set expression(mathJson: any): void
```

如果 Compute Engine 库可用，返回表示 mathfield 值的装箱 MathJSON 表达式。

要加载 Compute Engine 库，使用：

```js
import 'https://esm.run/@cortex-js/compute-engine';
```

##### MathfieldElement.value

```js
get value(): string
set value(value: string): void
```

mathfield 的内容，以 LaTeX 表达式表示。

```js
document.querySelector('mf').value = '\\frac{1}{\\pi}';
```

##### MathfieldElement.getValue()

###### getValue(format)

```js
getValue(format?): string
```

返回 mathfield 内容的文本表示。

###### format?

[`OutputFormat`](https://mathlive.io/mathfield/api/#outputformat)

结果的格式。如果使用 `math-json`，必须加载 Compute Engine 库，例如：

```js
import "https://esm.run/@cortex-js/compute-engine";
```

**默认值：** `"latex"`

###### getValue(start, end, format)

```js
getValue(start, end, format?): string
```

返回从 `start` 到 `end` 的 mathfield 值。

###### start

`number`

###### end

`number`

###### format?

[`OutputFormat`](https://mathlive.io/mathfield/api/#outputformat)

###### getValue(range, format)

```js
getValue(range, format?): string
```

返回 `range` 范围内的 mathfield 值。

###### range

[`Range`](https://mathlive.io/mathfield/api/#range-1)

###### format?

[`OutputFormat`](https://mathlive.io/mathfield/api/#outputformat)

##### MathfieldElement.insert()

```js
insert(s, options?): boolean
```

在当前插入点插入文本块。

此方法可以显式调用，也可以作为选择器通过 `executeCommand("insert")` 调用。

插入后，将根据 `options.selectionMode` 设置选区。

###### s

`string`

###### options?

[`InsertOptions`](https://mathlive.io/mathfield/api/#insertoptions)

##### MathfieldElement.setValue()

```js
setValue(value?, options?): void
```

将 mathfield 的内容设置为解释为 LaTeX 表达式的文本。

###### value?

`string`

###### options?

[`InsertOptions`](https://mathlive.io/mathfield/api/#insertoptions)

---

### 选区

##### MathfieldElement.lastOffset

最后一个有效偏移量。

##### MathfieldElement.position

```js
get position(): number
set position(offset: number): void
```

光标/插入点的位置，从 0 到 `lastOffset`。

##### MathfieldElement.selection

```js
get selection(): Readonly<Selection>
set selection(sel: number | Selection): void
```

表示选区的范围数组。

保证至少有一个元素。如果存在不连续的选区，结果将包含多个元素。

##### MathfieldElement.selectionIsCollapsed

##### MathfieldElement.getOffsetFromPoint()

```js
getOffsetFromPoint(x, y, options?): number
```

最接近视口坐标 `(x, y)` 位置的偏移量。

**`bias`**：如果为 `0`，则垂直中线被视为左侧或右侧兄弟元素。如果为 `-1`，则偏向左侧兄弟元素；如果为 `+1`，则偏向右侧兄弟元素。

###### x

`number`

###### y

`number`

###### options?

###### bias?

`-1` | `0` | `1`

##### MathfieldElement.select()

```js
select(): void
```

选中 mathfield 的全部内容。

---

### 自定义

##### MathfieldElement.restoreFocusWhenDocumentFocused

```js
static restoreFocusWhenDocumentFocused: boolean = true;
```

当从其他标签页切换到包含之前获得焦点的 mathfield 的标签页时，恢复 mathfield 的焦点。

此行为与 `<textarea>` 一致，但如果不需要，可以禁用它。

**默认值：** `true`

##### MathfieldElement.backgroundColorMap

```js
get backgroundColorMap(): (name) => string
set backgroundColorMap(value: (name) => string): void
```

##### MathfieldElement.colorMap

```js
get colorMap(): (name) => string
set colorMap(value: (name) => string): void
```

将 `\textcolor{}{}` 或 `\colorbox{}{}` 等命令中使用的颜色名称映射为 CSS 颜色值。

使用此选项覆盖"yellow"或"red"等颜色的标准映射。

如果名称不是您期望的，返回 `undefined`，将应用默认颜色映射。

如果未提供 `backgroundColorMap()` 函数，将使用 `colorMap()` 函数代替。

如果未提供 `colorMap()`，则应用默认颜色映射。

以下颜色名称已针对可读的前景色和背景色值进行了优化，推荐使用：

- `red`、`orange`、`yellow`、`lime`、`green`、`teal`、`blue`、`indigo`、`purple`、`magenta`、`black`、`dark-grey`、`grey`、`light-grey`、`white`

##### MathfieldElement.defaultMode

```js
get defaultMode(): "text" | "math" | "inline-math"
set defaultMode(value: "text" | "math" | "inline-math"): void
```

元素为空时的模式：

- `"math"`：相当于 `\displaystyle`（显示数学模式）
- `"inline-math"`：相当于 `\inlinestyle`（内联数学模式）
- `"text"`：文本模式

##### MathfieldElement.environmentPopoverPolicy

```js
get environmentPopoverPolicy(): "auto" | "off" | "on"
set environmentPopoverPolicy(value: "auto" | "off" | "on"): void
```

如果为 `"auto"`，则在显示虚拟键盘时，显示一个包含编辑环境（矩阵）命令的弹出面板。

**默认值：** `"auto"`

##### MathfieldElement.letterShapeStyle

```js
get letterShapeStyle(): "auto" | "tex" | "iso" | "french" | "upright"
set letterShapeStyle(value: "auto" | "tex" | "iso" | "french" | "upright"): void
```

控制字母形状样式：

| `letterShapeStyle` | xyz | ABC | αβɣ | ΓΔΘ |
|---|---|---|---|---|
| `iso` | 斜体 | 斜体 | 斜体 | 斜体 |
| `tex` | 斜体 | 斜体 | 斜体 | 正体 |
| `french` | 斜体 | 正体 | 正体 | 正体 |
| `upright` | 正体 | 正体 | 正体 | 正体 |

默认的字母形状样式是 `auto`，表示如果区域设置为"french"，则使用 `french`，否则使用 `tex`。

**历史说明**

"french"规则从何而来？TeX 标准字体 Computer Modern 基于 Monotype 155M，而 Monotype 155M 又基于 Porson 希腊字体，这是英语国家最广泛使用的希腊字体之一。该字体大写字母为正体，但小写字母为斜体。在法国，传统的希腊字体是 Didot，其大写和小写字母都是正体。

至于罗马大写字母，则遵循"Lexique des règles typographiques en usage à l'Imprimerie Nationale"的建议。需要注意的是，这一惯例并未被普遍遵循。

##### MathfieldElement.mathModeSpace

```js
get mathModeSpace(): string
set mathModeSpace(value: string): void
```

按下空格键（物理或虚拟键盘）时插入的 LaTeX 字符串。

使用 `"\;"` 表示厚空格，`"\:"` 表示中等空格，`"\,"` 表示薄空格。

不要使用 `" "`（常规空格），因为 LaTeX 会跳过空白，这不会产生任何效果。

**默认值：** `""`（空字符串）

##### MathfieldElement.maxMatrixCols

```js
get maxMatrixCols(): number
set maxMatrixCols(value: number): void
```

设置矩阵环境的最大列数。默认值为 10 列，与 amsmath 矩阵环境的行为一致。

**默认值：** `10`

##### MathfieldElement.minFontScale

```js
get minFontScale(): number
set minFontScale(value: number): void
```

设置嵌套上标和分数的相对最小字号。值应为 `0` 到 `1` 之间的数字。大小以相对 `em` 单位表示，相对于 `math-field` 元素的字号。指定值为 `0` 允许 `math-field` 使用其默认大小逻辑。

**默认值：** `0`

##### MathfieldElement.placeholder

```js
get placeholder(): string
set placeholder(value: string): void
```

当 mathfield 没有内容时显示的 LaTeX 字符串。

##### MathfieldElement.placeholderSymbol

```js
get placeholderSymbol(): string
set placeholderSymbol(value: string): void
```

用于表示表达式中占位符的符号。

**默认值：** `▢` `U+25A2 白色圆角方形`

##### MathfieldElement.popoverPolicy

```js
get popoverPolicy(): "auto" | "off"
set popoverPolicy(value: "auto" | "off"): void
```

如果为 `"auto"`，则在输入 LaTeX 命令时可能显示包含建议的弹出面板。

**默认值：** `"auto"`

##### MathfieldElement.removeExtraneousParentheses

```js
get removeExtraneousParentheses(): boolean
set removeExtraneousParentheses(value: boolean): void
```

如果为 `true`，将自动移除分子或分母周围多余的括号。

**默认值：** `true`

##### MathfieldElement.scriptDepth

```js
get scriptDepth(): number | [number, number]
set scriptDepth(value: number | [number, number]): void
```

此选项控制可以输入的下标/上标层数。例如，如果 `scriptDepth` 为 `1`，则可以有一层上标或下标。尝试在上标内输入上标将被拒绝。设置为 0 将阻止输入任何上标或下标（但不影响求和、积分等的上下限）。

这可以更轻松地输入符合 mathfield 使用领域预期的方程式。

要独立控制上标和下标的深度，提供一个数组：第一个元素表示下标的最大深度，第二个元素表示上标的深度。因此，值为 `[0, 1]` 将禁止输入下标，并允许一层上标。

##### MathfieldElement.smartFence

```js
get smartFence(): boolean
set smartFence(value: boolean): void
```

如果为 `true`，输入 `(` 将自动生成 `\left(`，输入 `[` 将生成 `\left[`，输入 `{` 将生成 `\left\{`。输入 `)`、`]` 或 `}` 将生成相应的 `\right` 分隔符。此行为在输入过程中提供视觉反馈，并确保正确嵌套。

**默认值：** `true`

##### MathfieldElement.smartMode

```js
get smartMode(): boolean
set smartMode(value: boolean): void
```

如果为 `true`，当检测到文本输入时（例如输入"if x > 0"），mathfield 将自动切换到文本模式。

**默认值：** `true`

##### MathfieldElement.smartSuperscript

```js
get smartSuperscript(): boolean
set smartSuperscript(value: boolean): void
```

如果为 `true`，在输入数字时自动退出上标模式。例如，输入 `x^2` 后输入数字将继续作为上标的一部分，但输入字母将退出上标模式。

**默认值：** `true`

##### MathfieldElement.inlineShortcutTimeout

```js
get inlineShortcutTimeout(): number
set inlineShortcutTimeout(value: number): void
```

内联快捷方式的超时时间（以毫秒为单位）。当用户输入一个单词后暂停，如果该单词匹配内联快捷方式，将被替换为相应的 LaTeX 命令。

**默认值：** `750`

##### MathfieldElement.readOnly

```js
get readOnly(): boolean
set readOnly(value: boolean): void
```

如果为 `true`，mathfield 的内容不可编辑，但仍可选择和复制。

**默认值：** `false`

##### MathfieldElement.disabled

```js
get disabled(): boolean
set disabled(value: boolean): void
```

如果为 `true`，mathfield 被禁用，内容不可编辑、不可选择，也不可复制。

**默认值：** `false`

---

### 命令执行

##### MathfieldElement.executeCommand()

```js
executeCommand(command): boolean
```

执行一个命令（选择器）。命令可以是字符串或数组。

```js
mf.executeCommand("selectAll");
mf.executeCommand(["insert", "\\frac"]);
```

了解更多关于[可用命令](https://mathlive.io/mathfield/guides/commands/)。

##### command

`string` | `any[]`

---

### 静态属性

##### MathfieldElement.locale

```js
static get locale(): string
static set locale(value: string): void
```

获取或设置 mathfield 的区域设置。影响所有 mathfield 实例的本地化字符串。

##### MathfieldElement.strings

```js
static get strings(): Record<string, Record<string, string>>
```

获取所有可用语言的本地化字符串。

##### MathfieldElement.decimalSeparator

```js
static get decimalSeparator(): string
static set decimalSeparator(value: string): void
```

获取或设置小数分隔符。可以是 `"."` 或 `","`。

---

### 输出格式

`OutputFormat` 类型定义了 `getValue()` 方法支持的输出格式：

- `"latex"`：LaTeX 格式
- `"math-json"`：MathJSON 格式（需要 Compute Engine 库）
- `"spoken"`：口语化文本
- `"text"`：纯文本

### 范围

`Range` 类型表示 mathfield 中的一个范围：

```ts
type Range = [start: number, end: number];
```

### 插入选项

`InsertOptions` 类型定义了插入操作的行为：

```ts
type InsertOptions = {
  selectionMode?: "placeholder" | "after" | "before" | "replace";
  format?: OutputFormat;
  insertionMode?: "replaceSelection" | "insertBefore" | "insertAfter";
};
```
