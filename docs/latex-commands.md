# LaTeX 命令

Mathfield 支持超过 800 个 LaTeX 命令。

> 如果想通过绘制符号来查找对应的 LaTeX 命令名称，可以使用 [Detexify](http://detexify.kirelabs.org/classify.html)。

要在 mathfield 中输入 LaTeX 命令，请按 **ESC** 键或 **\\** 键进入 LaTeX 编辑模式。再次按 **ESC** 键退出 LaTeX 编辑模式。

要查看某个表达式的 LaTeX 代码，选中它，然后按 **ESC** 键。

> 最常用的符号也可以通过[键盘快捷键](https://mathlive.io/mathfield/reference/keybindings/)输入。

---

## 文本区、数学区与数学样式

### 数学区

在数学区中，内容将使用数学专用的排版规则进行布局。

例如，变量（如 \(x\)）会以斜体显示，某些字母（如 \(f\)）周围会插入适当的间距以提高可读性，而空白字符则会被忽略。

在数学区中，某些数学元素的布局和大小会根据其使用上下文进行调整。例如，上标和下标会使用较小的字号：\(x^2_i\)。

在数学区内部，**数学样式**决定了用于显示内容的字体大小以及某些布局选项，例如求和或积分的上下限位置。

要覆盖默认的数学样式，请使用以下命令：

| 命令 | 示例 |
|------|------|
| `\displaystyle` 用于独立段落的公式 | `\displaystyle \sum_{i=0}^n \frac{a_i}{1+x}` |
| `\textstyle` 用于行内数学（注意：不是用于文本内容） | `\textstyle \sum_{i=0}^n \frac{a_i}{1+x}` |
| `\scriptstyle` 用于下标和上标 | `\scriptstyle \sum_{i=0}^n \frac{a_i}{1+x}` |
| `\scriptscriptstyle` 用于下标的上标或上标的下标 | `\scriptscriptstyle \sum_{i=0}^n \frac{a_i}{1+x}` |

### 文本区

要包含一些文本内容，请使用 `\text{}` 或 `\textrm{}` 命令切换到文本区。在文本区中，空白字符会被保留，字符间距不会被调整。

- `\text{if and only if } x > 0` — 当且仅当 \(x > 0\)
- `\text{Donald Knuth created LaTeX}` — `\text{}` 命令将使用封闭 mathfield 的 CSS `font-family` 属性定义的字体。文本大小会根据当前数学样式进行调整（在上标/下标中会更小）。
- `\textrm{Donald Knuth is the author of "The Art of Computer Programming"}` — `\textrm{}` 命令与 `\text{}` 类似，但会使用衬线（罗马）字体。
- `\mbox{Donald Knuth received the Turing Award in 1974}` — `\mbox{}` 命令使用与 `\text` 相同的字体，但其大小不会随当前数学样式变化。
- `\textnormal{Donald Knuth is a Professor Emeritus at Stanford University}` — `\textnormal{}` 命令与 `\text{}` 类似，但输入更长。

在文本区中，使用 `$...$` 可以切换回行内数学区，使用 `\\[...\\]` 可以切换到显示（块级）数学区。

---

## 分数与二项式系数

`\frac` 命令用于表示分数。第一个参数是分子，第二个参数是分母。它会根据当前数学样式（显示、文本/行内、脚本、脚本脚本）自动调整大小。`\dfrac` 和 `\tfrac` 命令分别强制使用显示样式或文本（行内）样式。

`\cfrac`（连分数）命令有一个可选参数 `[l]` 或 `[r]`，用于控制分子是左对齐还是右对齐。

- `\frac{}{}` `\dfrac{}{}` `\tfrac{}{}` `\cfrac[l]{}{}` `\cfrac[r]{}{}`

`\pdiff` 命令是偏微分的便捷简写。

- `\pdiff{}{}`

`\binom` 命令用于表示二项式系数。`\dbinom` 和 `\tbinom` 命令分别强制使用显示样式或文本（行内）样式。

- `\binom{}{}` `\dbinom{}{}` `\tbinom{}{}`

> **已弃用**
> 以下命令虽然支持，但在创建现代 LaTeX 内容时通常不推荐使用。
>
> `a \over b` `a \atop b` `a \choose b` `\overwithdelims\lbrace\rbrace` `\atopwithdelims\lbrace\rbrace`

---

## 二元运算符

某些二元运算符也可以用作一元运算符：`+`、`-` 等。它们的间距会相应调整。例如，在 \(-1 + 2\) 中，`-` 和 `1` 之间的间距比 `-` 和 `2` 之间的间距更小。

`+` `-` `\pm` `\mp` `a / b` `\nicefrac{3}{4}` `\div` `\divides` `\sqrt{}` `\sqrt[]{}` `\surd{}` `\intercal` `\dotplus` `\doublebarwedge` `\divideontimes` `\times` `\cdot` `*` `\ast` `\star` `\ltimes` `\rtimes` `\leftthreetimes` `\rightthreetimes` `\circ` `\bullet` `\centerdot` `\boxminus` `\boxplus` `\boxtimes` `\boxdot` `\ominus` `\oplus` `\otimes` `\odot` `\circleddash` `\circledast` `\circledcirc` `\oslash`

---

## 函数

`\exp` `\ln` `\log` `\lg` `\lb` `\ker` `\det` `\arg` `\dim` `\gcd` `\argmin` `\argmax` `\plim`

### 三角函数

`\degree` `^\circ` `\ang{}` `\arccos` `\arcsin` `\arctan` `\cos` `\cosh` `\cot` `\coth` `\csc` `\sec` `\sin` `\sinh` `\tan` `\tanh`

### 非标准三角函数

本节中的命令不属于标准 LaTeX 发行版，但某些宏包提供了它们。请谨慎使用，因为它们可能不被所有 LaTeX 引擎支持。建议改用 `\operatorname{}`。

`\arctg` `\arcctg` `\ch` `\ctg` `\cth` `\cotg` `\cosec` `\sh` `\tg` `\th`

### 界限

`\max` `\min` `\sup` `\inf` `\lim` `\liminf` `\limsup` `\injlim` `\varlimsup` `\varliminf` `\varinjlim`

### 投影

`\Pr` `\hom` `\varprojlim` `\projlim`

### 模运算

- `n \pmod{3}` — 带括号的模运算
- `n \mod{3}` — 不带括号的模运算
- `n \bmod 3` — 二元模运算

### 自定义函数

要定义自定义函数，请使用 `\operatorname{}` 命令：函数名称将以正体显示并带有适当的间距。

- `\operatorname{argth}(\theta)`

---

## Unicode

如果某个符号没有对应的 LaTeX 命令，可以使用该字符的 Unicode 码点。以下命令可用于在 mathfield 中插入 Unicode 字符。

| 命令 | 说明 |
|------|------|
| `\unicode{}` | 参数是以数字表示的 Unicode 码点。要使用十六进制数，参数以 `x` 或 `"` 开头，十六进制数字使用大写 A-F。例如：`\unicode{10775}` `\unicode{"2A17}` `\unicode{x2A17}` |
| `\char` | 参数也是 Unicode 码点，但使用 `"` 时 `{...}` 定界符可选。例如：`\char"2A17` |
| `^^` `^^^^` | 后跟 2 个或 4 个十六进制数字（小写 a-f）来指定 Unicode 码点。例如：`^^4a` `^^^^2a17` |

> **注意：** Unicode 字符 ⨗（U+2A17，带左钩箭头的积分）的码点十进制为 10775，十六进制为 2A17。字母 `J` 的码点十六进制为 004A。更多信息请参阅 Wikipedia 上的[数学运算符和符号（Unicode）](https://en.wikipedia.org/wiki/Mathematical_operators_and_symbols_in_Unicode)。

---

## 大型运算符

大型运算符会根据数学样式（显示样式或文本样式）以及运算符本身，将其上下限显示在运算符的上方和下方或相邻位置。

可以使用 `\limits`、`\nolimits` 或 `\displaylimits` 来控制上下限的位置。`\limits` 强制将上下限显示在运算符的上方和下方，`\nolimits` 强制将上下限显示在运算符的相邻位置，`\displaylimits` 则根据运算符和当前数学样式自动选择位置。

| `\sum_{i=0}^n\limits` | `\sum_{i=0}^n\nolimits` | `\sum_{i=0}^n\displaylimits` |
|------------------------|--------------------------|-------------------------------|
| `\int_0^\infty\limits` | `\int_0^\infty\nolimits` | `\int_0^\infty\displaylimits` |

在显示样式中，`\intop` 和 `\ointop` 命令默认将上下限显示在上方和下方，而 `\int` 命令则将上下限显示在相邻位置。

`\sum` `\prod` `\coprod` `\int` `\intop` `\iint`（二重积分） `\iiint`（三重积分） `\oint`（围道积分） `\smallint`（始终显示为小号） `\bigcup` `\bigcap` `\bigvee` `\bigwedge` `\biguplus` `\bigotimes` `\bigoplus` `\bigodot` `\bigsqcup` `\oiint`（曲面积分） `\oiiint`（体积积分） `\intclockwise` `\varointclockwise` `\ointctrclockwise` `\intctrclockwise` `\Cap` `\Cup` `\doublecap` `\doublecup` `\sqcup` `\sqcap` `\uplus` `\wr` `\amalg`

### 逻辑符号

#### 量词

`\forall` `\exists` `\nexists`

#### 一元/二元运算符

`\land` `\wedge` `\lor` `\vee` `\barwedge` `\veebar` `\nor` `\curlywedge` `\curlyvee` `\lnot` `\neg`

#### 关系运算符

`\to` `\gets` `\implies` `\impliedby` `\biconditional` `\therefore` `\because` `\leftrightarrow` `\Leftrightarrow` `\roundimplies` `\models` `\vdash` `\dashv`

---

## 箭头

`\rightarrow` `\leftarrow` `\twoheadrightarrow` `\twoheadleftarrow` `\rightarrowtail` `\leftarrowtail` `\dashrightarrow` `\dashleftarrow` `\longrightarrow` `\longleftarrow` `\longleftrightarrow` `\Rightarrow` `\Leftarrow` `\Longrightarrow` `\Longleftarrow` `\Longleftrightarrow` `\mapsto` `\longmapsto` `\multimap` `\uparrow` `\downarrow` `\updownarrow` `\Uparrow` `\Downarrow` `\Updownarrow` `\rightharpoonup` `\leftharpoonup` `\rightharpoondown` `\leftharpoondown` `\rightleftharpoons` `\leftrightharpoons` `\searrow` `\nearrow` `\swarrow` `\nwarrow` `\Rrightarrow` `\Lleftarrow` `\leftrightarrows` `\rightleftarrows` `\curvearrowright` `\curvearrowleft` `\hookrightarrow` `\hookleftarrow` `\looparrowright` `\looparrowleft` `\circlearrowright` `\circlearrowleft` `\rightrightarrows` `\leftleftarrows` `\upuparrows` `\downdownarrows` `\Rsh` `\Lsh` `\upharpoonright` `\upharpoonleft`

### 否定箭头

`\nrightarrow` `\nleftarrow` `\nleftrightarrow` `\nRightarrow` `\nLeftarrow` `\nLeftrightarrow`

### 可伸缩箭头

上述箭头命令的长度是固定的。本节中的命令长度由箭头上下方内容的长度决定，通过参数（和可选参数）指定。

`\xrightarrow[\text{long text below}]{}\) `\xrightarrow{\text{long text above}}` `\xrightarrow[\text{and below}]{\text{long text above}}` `\xlongequal[]{}` `\xrightarrow[]{}` `\xleftarrow[]{}` `\xleftrightarrow[]{}` `\xtwoheadrightarrow[]{}` `\xtwoheadleftarrow[]{}` `\xRightarrow[]{}` `\xLeftarrow[]{}` `\xrightharpoonup[]{}` `\xleftharpoonup[]{}` `\xrightharpoondown[]{}` `\xleftharpoondown[]{}` `\xrightleftharpoons[]{}` `\xleftrightharpoons[]{}` `\xhookrightarrow[]{}` `\xhookleftarrow[]{}` `\xmapsto[]{}` `\xtofrom[]{}`

---

## 重音

`\dot` `\ddot` `\dddot` `\ddddot` `\mathring` `\tilde` `\bar` `\breve` `\check` `\hat` `\vec`

### 已弃用的重音

> **已弃用**
> 以下命令为了兼容现有内容而支持，但在创建新 LaTeX 内容时通常不推荐使用（当存在等效的 Unicode 字符时）。
> 例如，使用 `é` 而不是 `\'{e}`。

`\acute` `\grave` `\^` `` \` `` `\'` `\"` `\=` `\c` `\~`

### 可伸缩重音

普通重音具有固定宽度，不会拉伸。例如，比较：

`\vec{ABC}` `\overrightarrow{ABC}` `\overline{ABC}` `\overgroup{ABC}` `\overbrace{ABC}` `\overlinesegment{ABC}` `\overrightarrow{ABC}` `\overleftarrow{ABC}` `\overleftrightarrow{ABC}` `\overarc{ABC}` `\overparen{ABC}` `\wideparen{ABC}` `\widetilde{ABC}` `\widehat{ABC}` `\widecheck{ABC}` `\Overrightarrow{ABC}` `\overleftharpoon{ABC}` `\overrightharpoon{ABC}` `\underline{ABC}` `\undergroup{ABC}` `\underbrace{ABC}` `\underlinesegment{ABC}` `\underrightarrow{ABC}` `\underleftarrow{ABC}` `\underleftrightarrow{ABC}` `\underarc{ABC}` `\underparen{ABC}` `\utilde{ABC}`

---

## 关系运算符

要显示两个符号的垂直"堆叠"作为关系运算符，请使用 `\stackrel` 命令：`a\stackrel{?}{=}b`。

`=` `<` `\lt` `>` `\gt` `\le` `\leq` `\ge` `\geq` `\shortparallel` `\leqslant` `\geqslant` `\gtrsim` `\approxeq` `\thickapprox` `\lessapprox` `\gtrapprox` `\precapprox` `\succapprox` `\thicksim` `\succsim` `\precsim` `\backsim` `\eqsim` `\backsimeq` `\lesssim` `\smallsmile` `\smallfrown` `\leqq` `\eqslantless` `\lll` `\lessgtr` `\lesseqgtr` `\lesseqqgtr` `\risingdotseq` `\fallingdotseq` `\preccurlyeq` `\curlyeqprec` `\vDash` `\Vvdash` `\bumpeq` `\Bumpeq` `\geqq` `\eqslantgtr` `\ggg` `\gtrless` `\gtreqless` `\gtreqqless` `\succcurlyeq` `\curlyeqsucc` `\Vdash` `\shortmid` `\between` `\pitchfork` `\varpropto` `\llless` `\gggtr` `\doteqdot` `\Doteq` `\eqcirc` `\circ`

### 否定关系运算符

要否定其他关系运算符，请使用 `\not` 命令，例如 `\not\equiv`。

`\ne` `\neq` `\not=` `\not` `\nless` `\nleq` `\lneq` `\lneqq` `\nleqq` `\nleqslant` `\ngeq` `\lvertneqq` `\lnsim` `\lnapprox` `\nprec` `\npreceq` `\precnsim` `\precnapprox` `\nsim` `\nshortmid` `\nmid` `\nvdash` `\nvDash` `\ngtr` `\ngeqslant` `\ngeqq` `\gneq` `\gneqq` `\gvertneqq` `\gnsim` `\gnapprox` `\nsucc` `\nsucceq` `\succnsim` `\succnapprox` `\ncong` `\nshortparallel` `\nparallel` `\nVDash` `\nVdash` `\precneqq` `\succneqq` `\unlhd` `\unrhd`

---

## 集合

`\emptyset` `\varnothing`

要表示自然数、整数、实数等集合，建议使用 `\mathbb` 命令以获得最佳兼容性，例如 `\mathbb{N}` 或 `\mathbb{C}` 等。

非标准命令，可能不被所有 LaTeX 引擎支持：

`\N` `\R` `\Q` `\C` `\Z` `\P` `\doubleStruckCapitalN` `\doubleStruckCapitalR` `\doubleStruckCapitalQ` `\doubleStruckCapitalZ` `\doubleStruckCapitalP`

### 集合运算符

`\cap` `\cup` `\setminus` `\smallsetminus` `\complement`

### 集合关系运算符

`\nsupseteqq` `\supsetneq` `\varsupsetneq` `\supsetneqq` `\varsupsetneqq` `\nsubseteqq` `\subseteqq` `\Subset` `\sqsubset` `\supseteqq` `\Supset` `\sqsupset` `\sqsubseteq` `\sqsupseteq` `\in` `\notin` `\ni` `\owns` `\backepsilon` `\subset` `\supset` `\subseteq` `\supseteq` `\subsetneq` `\varsubsetneq` `\subsetneqq` `\varsubsetneqq` `\nsubset` `\nsupset` `\nsubseteq` `\nsupseteq`

---

## 希腊字母

`\alpha` `\beta` `\gamma` `\delta` `\epsilon` `\varepsilon` `\zeta` `\eta` `\theta` `\vartheta` `\iota` `\kappa` `\varkappa` `\lambda` `\mu` `\nu` `\xi` `\omicron` `\pi` `\varpi` `\rho` `\varrho` `\sigma` `\varsigma` `\tau` `\phi` `\varphi` `\upsilon` `\chi` `\psi` `\omega` `\digamma`

`\Alpha` `\Beta` `\Gamma` `\varGamma` `\Delta` `\varDelta` `\Epsilon` `\Zeta` `\Eta` `\Theta` `\varTheta` `\Iota` `\Kappa` `\Lambda` `\varLambda` `\Mu` `\Nu` `\Xi` `\varXi` `\Omicron` `\Pi` `\varPi` `\Rho` `\Sigma` `\varSigma` `\Tau` `\Phi` `\varPhi` `\Upsilon` `\varUpsilon` `\Chi` `\Psi` `\varPsi` `\Omega` `\varOmega`

---

## 希伯来字母

`\aleph` `\beth` `\gimel` `\daleth`

---

## 类字母符号

`@` `\mid` `\top` `\bot` `\nabla` `\partial` `\ell` `\hbar` `\pounds` `\euro` `\And` `\$` `\%` `\differencedelta` `\wp` `\hslash` `\Finv` `\Game` `\eth` `\mho` `\Bbbk` `\yen` `\imath` `\jmath` `\degree` `\Re` `\Im`

---

## 定界符

定界符（也称为 fences）是用于分组符号的符号，例如括号、方括号、花括号等。

要根据内容自动调整定界符大小，请使用 `\left...\right`。

| 不使用 `\left...\right` | 使用 `\left...\right` |
|--------------------------|------------------------|
| `\lbrace x \| \frac{x}{2} > 0\rbrace` | `\left\lbrace x \middle\| \frac{x}{2} > 0\right\rbrace` |

左右定界符不必匹配：

- `\left\lparen \frac1x \right\rbrack`

要省略定界符，请使用 `.`：

- `\left\lparen \frac1x \right.`

`\left`、`\right` 和 `\middle` 的参数可以是以下命令之一：

`\lparen` `\rparen` `\lbrace` `\rbrace` `\langle` `\rangle` `\lfloor` `\rfloor` `\lceil` `\rceil` `\vert` `\lvert` `\rvert` `\|` `\Vert` `\mVert` `\lVert` `\rVert` `\lbrack` `\rbrack` `\{` `\}` `(` `)` `[` `]` `\ulcorner` `\urcorner` `\llcorner` `\lrcorner` `\lgroup` `\rgroup` `\lmoustache` `\rmoustache` `\mvert`

---

## 标点符号

`.` `?` `!` `:` `\Colon` `\colon` `,` `;` `"`

### 省略号/点

`\cdotp` `\ldotp` `\vdots` `\cdots` `\ddots` `\ldots` `\mathellipsis`

---

## 形状

`\square` `\Box` `\blacksquare` `\bigcirc` `\circledS` `\circledR` `\diamond` `\Diamond` `\lozenge` `\blacklozenge` `\triangleleft` `\triangleright` `\triangle` `\triangledown` `\blacktriangleleft` `\blacktriangleright` `\blacktriangle` `\blacktriangledown` `\vartriangle` `\vartriangleleft` `\vartriangleright` `\triangleq` `\trianglelefteq` `\trianglerighteq` `\ntriangleleft` `\ntriangleright` `\ntrianglelefteq` `\ntrianglerighteq` `\bigtriangleup` `\bigtriangledown` `\dagger` `\dag` `\ddag` `\ddagger` `\maltese`

---

## St Mary's Road 理论计算机科学符号

`\mapsfrom` `\Mapsfrom` `\MapsTo` `\Yup` `\lightning` `\leftarrowtriangle` `\rightarrowtriangle` `\leftrightarrowtriangle` `\boxdot` `\bigtriangleup` `\bigtriangledown` `\boxbar` `\Lbag` `\Rbag` `\llbracket` `\rrbracket` `\longmapsfrom` `\Longmapsfrom` `\Longmapsto` `\boxslash` `\boxbslash` `\boxast` `\boxcircle` `\boxbox` `\fatsemi` `\leftslice` `\rightslice` `\interleave` `\biginterleave` `\sslash` `\talloblong`

---

## 布局

这些命令用于改变符号周围的间距：

- `\mathop{}` — 将其参数视为大型运算符
- `\mathrel{}` — 将其参数视为关系运算符
- `\mathbin{}` — 将其参数视为二元运算符
- `\mathopen{}` 和 `\mathclose{}` — 分别视为开定界符和闭定界符
- `\mathpunct{}` — 视为标点符号
- `\mathinner{}` — 视为分数
- `\mathord{}` — 视为普通符号

`x\mathop{+}0+1` `x=\mathbin{arg}=0` `x=\mathrel{arg}=0` `x=\mathopen{arg}=0` `x=\mathclose{arg}=0` `x=\mathpunct{arg}=0` `x=\mathinner{arg}=0` `x=\mathord{arg}=0` `\overset{arg}{x=0}` `\underset{arg}{x=0}` `\overunderset{arg}{x=0}{y=1}` `\stackrel{arg}{x=0}` `\stackbin{arg}{x=0}` `\rlap{/}0` `o\llap{/}` `o\mathllap{/}` `\mathrlap{/}0`

---

## 间距

`\hspace` `\hspace*` `\!` `\,` `:` `;` `\enskip` `\enspace` `\quad` `\qquad`

---

## 装饰

### 符号标注

#### `\enclose`

`\enclose` 命令非常灵活。它接受三个参数，其中两个是必需的：

```
\enclose{<notation>}[<style>]{<body>}
```

- `<notation>`：以空格分隔的值列表：
  - `box` `roundedbox` `circle` `top` `left` `bottom` `right` `horizontalstrike` `verticalstrike` `updiagonalstrike` `downdiagonalstrike` `updiagonalarrow` `phasorangle` `radical` `longdiv` `actuarial` `madruwb`
  
  它们可以组合使用：`\enclose{roundedbox updiagonalstrike}{x=0}`

- `<style>`：可选的逗号分隔的键值对列表，包括：
  - `mathbackground="<color>"` — 表达式的背景颜色
  - `mathcolor="<color>"` — 标注的颜色，例如 `red` 或 `#cd0030` 或 `rgba(205, 0, 11, .4)`
  - `padding="<dimension>"` — `"auto"` 或内容周围的填充量
  - `shadow="<shadow>"` — `"auto"` 或 `"none"` 或 CSS `box-shadow` 表达式，例如 `"0 0 2px rgba(0, 0, 0, 0.5)"`
  - 此外，样式属性可以包含遵循 CSS `border` 属性简写语法的描边样式表达式，例如 `"2px solid red"`

- `<body>`：被指定标注"包围"的数学表达式

> **注意：** `\enclose` 是 MathJax 引入的 LaTeX 扩展，遵循 MathML 的 `<menclose>` 定义。

示例：
- `\enclose{updiagonalstrike roundedbox}[1px solid red, mathbackground="#fbc0bd"]{x=0}`
- `\enclose{circle}[mathbackground="#fbc0bd"]{\frac1x}`
- `\enclose{roundedbox}[1px dotted #cd0030]{\frac{x^2+y^2}{\sqrt{x^2+y^2}}}`

#### `\cancel`、`\bcancel` 和 `\xcancel`

| 命令 | 等效于 |
|------|--------|
| `\cancel{body}` | `\enclose{updiagonalstrike}{body}` |
| `\bcancel{body}` | `\enclose{downdiagonalstrike}{body}` |
| `\xcancel{body}` | `\enclose{updiagonalstrike downdiagonalstrike}{body}` |

> **注意：** `\cancel`、`\bcancel` 和 `\xcancel` 命令来自 [cancel](https://www.ctan.org/pkg/cancel) LaTeX 宏包。

### 快捷键

一些命令是常见标注的快捷键：

| 命令 | 等效于 |
|------|--------|
| `\angl{body}` | `\enclose{actuarial}{body}` |
| `\angln` | `\enclose{actuarial}{n}` |
| `\anglr` | `\enclose{actuarial}{r}` |
| `\anglk` | `\enclose{actuarial}{k}` |

### 颜色

要更改前景色，请使用 `\textcolor{}{}` 命令。

要更改背景色，请使用 `\colorbox{}{}` 命令。

这些命令的第一个参数是颜色，可以指定为：

- 以下之一：`red`、`orange`、`yellow`、`lime`、`green`、`teal`、`blue`、`indigo`、`purple`、`magenta`、`black`、`dark-grey`、`grey`、`light-grey`、`white`
- 使用标准 CSS 格式的 RGB 颜色（`#d7170b` 或 `rgb(240, 20, 10)`）
- [dvips 颜色名称](https://ctan.org/pkg/colordvi)中的 68 种颜色之一（`CadetBlue`）。注意这些名称区分大小写。
- Mathematica 的 `ColorData[97, "ColorList"]` 中的 10 种颜色之一（`M0` 到 `M9`）
- 使用 [xcolor 宏包](http://mirror.jmu.edu/pub/CTAN/macros/latex/contrib/xcolor/xcolor.pdf)语法定义的颜色，例如：`Blue!20!Black!30!Green`
- 如果颜色前缀为 `-`，则使用互补色

推荐使用以下颜色名称：

![推荐颜色](https://mathlive.io/assets/images/colors-6d67468bbc947b06d4fc602441d7d116.png)

> **注意：** 这些颜色经过精心挑选，在色环上具有平衡的色相范围，且亮度和强度相似。它们映射到的颜色值与同名的 `dvips` 颜色不同。

> **注意：** 为确保基于使用的可读性，这些颜色名称在前景色和背景色使用时将映射到不同的值。要使用特定的颜色值，请改用 RGB 颜色。

> **注意：** 为了在不同 TeX 版本之间获得最佳可移植性，建议将 DVIPS 颜色限制为以下子集：`White`、`Black`、`Gray`、`Red`、`Orange`、`Yellow`、`LimeGreen`、`Green`、`TealBlue`、`Blue`、`Violet`、`Purple` 和 `Magenta`。这些名称区分大小写。

- `\textcolor{blue}{x+1=0}` — 推荐使用，优于 `\color`
- `{\color{blue} x+1=0}`
- `\colorbox{yellow}{\[ax^2+bx+c\]}` — 参数在文本模式中。使用 `\\[...\\]` 切换到数学模式
- `\fcolorbox{#cd0030}{#ffd400}{\unicode{"2B1A}}`
- `\boxed{1+\frac{1}{x}}`
- `\bbox[]{\unicode{"2B1A}}` — 参见 MathJax BBox 文档
- `\rule[]{2em}{1em}` — 参数分别为宽度和高度。可选参数是基线偏移量。

---

## 字体样式

### 粗体

`\fontseries{b}` `\boldsymbol{}` `{\bfseries}` `{\mdseries}` `\bm{}` `\bold{}` `\textbf{}` `\textmd{}` `\mathbf{}` `\mathbfit{}`

### 斜体

`\upshape` `\slshape` `\textup{}`（正体） `\textsl{}`（倾斜体） `\textit{}`（斜体） `\mathit{}`（数学斜体）

### 字体系列

#### 等宽/打字机字体

`\fontfamily{}` `\texttt{}` `\mathtt{}` `\ttfamily` `{\ttfamily ABCabc}`

#### 无衬线字体

`\textsf{}` `\mathsf{}` `\sffamily`

#### 数学变体

- `\mathfrak{ABCdef}` —  Fraktur（德文尖角体）
- `\mathcal{ABCdef}` — 书法体
- `\mathscr{ABCdef}` — 手写体
- `\mathbb{}` — 黑板粗体
- `\Bbb{}` — 黑板粗体
- `{\frak}{\frak ABC}`
- `\text{\rmfamily}`

### MathJax HTML 扩展

Mathfield 支持 [MathJax HTML 扩展](http://docs.mathjax.org/en/latest/input/tex/extensions/html.html)中的一些命令。

#### `\class{className}{expression}`

```html
\class{custom-CSS-class}{x+1}
```

在 `<math-field>` 组件中使用时，类名应引用在 `<math-field>` 元素内部使用 `<style>` 标签定义的样式表。

```html
<math-field>
  <style>
    #custom-CSS-class {
      box-shadow: 0 0 10px #000;
      border-radius: 8px;
      padding: 8px;
    }
  </style>
  \class{custom-CSS-class}{\frac{1}{x+1}}
</math-field>
```

#### `\cssId{id}{expression}`

为表达式应用元素 ID。然后可以使用 CSS 对该元素进行样式设置。

```css
#custom-CSS-class {
  box-shadow: 0 0 4px #999;
  border-radius: 8px;
  padding: 8px;
}
```

```
\cssId{custom-CSS-class}{\text{Don Knuth}}
```

#### `\htmlData{key=value,...}{content}`

该命令的参数是逗号分隔的键/值对列表，例如 `\htmlData{foo=green,bar=blue}{x=0}`。对应的 `foo` 和 `bar` DOM 属性会附加到渲染的 DOM 元素上。

```
\htmlData{foo=green,bar=blue}{ \text{Don Knuth} }
```

#### `\href{url}{content}`

第一个参数是 URL，第二个参数是要显示的内容（数学模式）。

```html
<math-field>
  \href{https://mathlive.io}{\text{MathLive website}}
</math-field>
```

### 其他扩展

#### `\error{content}`

该命令的参数是一个字符串，将以红色背景和红色下划线渲染。

```
\text{Don \error{\text{Knuht}}}
```

#### `\texttip{expression}{hover text}`

第一个参数是要显示的数学表达式，第二个参数是悬停时显示的文本。

```
\texttip{e^{i\pi}-1=0}{The most beautiful equation}
```

#### `\mathtip{expression}{hover math expression}`

第一个参数是要显示的数学表达式，第二个参数是悬停时显示的数学表达式。

```
\mathtip{e^{i\pi}}{e^{i\pi}=1}
```

### 其他

- `\text{\fontshape{sc}Don Knuth}` — 小型大写字母
- `{\scshape Don Knuth}` — 小型大写字母
- `\textsc{Don Knuth}` — 小型大写字母
- `\textrm{Don Knuth}` — 罗马体
- `\mathrm{Don Knuth}` — 罗马体
- `\text{Don {\em Knuth}}` — 强调
- `\text{Don \emph{Knuth}}` — 强调

> **已弃用**
> 以下命令为了兼容现有内容而支持，但在创建新 LaTeX 内容时通常不推荐使用。
>
> - `{\bf Don Knuth}` — 改用 `\textbf{}` 或 `\bfseries`
> - `{\it Don Knuth}` — 改用 `\textit{}` 或 `\itshape`

`\selectfont`

---

## 大小

在 LaTeX 中，使用以下大小命令可能并不总能达到预期效果。在 mathfield 中，这些大小命令会一致地应用于文本和数学模式。

这些大小相对于 mathfield 的 `font-size` 属性。

`\tiny{e^{i\pi}+1=0}` `\scriptsize{e^{i\pi}+1=0}` `\footnotesize{e^{i\pi}+1=0}` `\small{e^{i\pi}+1=0}` `\normalsize{e^{i\pi}+1=0}` `\large{e^{i\pi}+1=0}` `\Large{e^{i\pi}+1=0}` `\LARGE{e^{i\pi}+1=0}` `\huge{e^{i\pi}+1=0}` `\Huge{e^{i\pi}+1=0}`

定界符的大小可以使用以下命令手动控制。`\left...\right...` 命令会根据内容自动计算定界符的大小。

> **警告：** 命令后必须跟一个定界符，例如 `(` 或 `\lbrace` 或 `\lbrack`。如果单独使用该命令，则不会显示任何内容。

`\bigl(` `\bigm\|` `\bigr)` `\Bigl(` `\Bigm\|` `\Bigr)` `\biggl(` `\biggm\|` `\biggr)` `\Biggl(` `\Biggm\|` `\Biggr)`

---

## 杂项

`\infty` `\prime` `\doubleprime` `/` `\/` `|` `\backslash` `\diagup` `\sharp` `\flat` `\natural` `\#` `\&` `\clubsuit` `\heartsuit` `\spadesuit` `\diamondsuit` `\angle` `\_` `\checkmark` `\measuredangle` `\sphericalangle` `\backprime` `\backdoubleprime` `\originalof` `\laplace` `\imageof` `\Laplace` `−` `` ` `` `~` `\space` `\smash[]{}` `\vphantom{}` `\hphantom{}` `\phantom{}`

---

## MediaWiki（`texvc.sty` 宏包）

Mathfield 支持 [MediaWiki](https://en.wikipedia.org/wiki/Help:Displaying_a_formula) 页面使用的命令（已弃用的除外）。

`\darr` `\dArr` `\Darr` `\lang` `\rang` `\uarr` `\uArr` `\Uarr` `\N` `\R` `\Z` `\alef` `\alefsym` `\Alpha` `\Beta` `\bull` `\Chi` `\clubs` `\cnums` `\Complex` `\Dagger` `\diamonds` `\empty` `\Epsilon` `\Eta` `\exist` `\harr` `\hArr` `\Harr` `\hearts` `\image` `\infin` `\Iota` `\isin` `\Kappa` `\larr` `\lArr` `\Larr` `\lrarr` `\lrArr` `\Lrarr` `\Mu` `\natnums` `\Nu` `\Omicron` `\plusmn` `\rarr` `\rArr` `\Rarr` `\real` `\reals` `\Reals` `\Rho` `\sdot` `\sect` `\spades` `\sub` `\sube` `\supe` `\Tau` `\thetasym` `\weierp` `\Zeta`

---

## 物理

### 括号符号

Mathfield 支持 [braket 宏包](https://ctan.org/pkg/braket)的命令。

- `\bra{\Psi}` — 左矢（bra）
- `\ket{\Psi}` — 右矢（ket）
- `\braket{ab}` — 内积
- `\Bra{ab}` — 左矢（大写）
- `\Ket{ab}` — 右矢（大写）
- `\Braket{ab}` — 内积（大写）

---

## 化学（`mhchem` 宏包）

Mathfield 支持 [mhchem 宏包](https://mhchem.github.io/MathJax-mhchem/)的命令。

### 化学式

- `\ce{H2O}` — 水
- `\ce{Sb2O3}` — 三氧化二锑

### 电荷

- `\ce{[AgCl2]-}`
- `\ce{Y^99+}` `\ce{Y^{99+}}`
- `\ce{H+}`
- `\ce{CrO4^2-}`

### 化学计量数

- `\ce{2 H2O}` `\ce{2H2O}`
- `\ce{0.5 H2O}`
- `\ce{1/2 H2O}`
- `\ce{(1/2) H2O}`
- `\ce{ H2O}`

### 同位素

- `\ce{^{227}_{90}Th+}` `\ce{^227_90Th+}`
- `\ce{^{0}_{-1}n^{-}}` `\ce{^0_-1n-}`
- `\ce{H{}^3HO}` `\ce{H^3HO}`

### 复杂示例

- `\ce{CO2 + C -> 2 CO}`
- `\ce{Hg^2+ ->[I-] HgI2 ->[I-] [Hg^{II}I4]^2-}`
- `\ce{$K = \ce{\frac{[Hg^2+][Hg]}{[Hg2^2+]}}$}`
- `\ce{Hg^2+ ->[I-]  $\underset{\mathrm{red}}{\ce{HgI2}}$  ->[I-]  $\underset{\mathrm{red}}{\ce{[Hg^{II}I4]^2-}}$}`

---

## 宏

`\iff` `\set{ab}` `\rd` `\rD` `\scriptCapitalE` `\scriptCapitalH` `\scriptCapitalL` `\gothicCapitalC` `\gothicCapitalH` `\gothicCapitalI` `\gothicCapitalR` `\imaginaryI` `\imaginaryJ` `\exponentialE` `\differentialD` `\capitalDifferentialD` `\Set{ x\in\mathbf{R}^2 \;\middle|\; 0<{|x|}<5 }`

---

## 环境/矩阵

环境用于排版一组相关项目，例如矩阵中的单元格或多行方程。

表格环境中的每一行由 `\\` 命令分隔。每一列由 `&` 分隔。

### 矩阵

#### `array`

一个没有定界符的简单表格。

```
\begin{array}{lc}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{array}
```

`{lc}` 参数指定列数及其格式：
- `l`：左对齐
- `c`：居中对齐
- `r`：右对齐

要在列之间添加竖线，请在列格式中添加 `|` 字符：

```
\begin{array}{l|c}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{array}
```

要添加双竖线，请在列格式中添加两个 `|` 字符：

```
\begin{array}{l||c}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{array}
```

要在两列之间添加虚线竖线，请使用 `:`：

```
\begin{array}{l:c}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{array}
```

#### `matrix`

`matrix` 环境与 `array` 非常相似，但它没有指定列格式的参数。

```
\begin{matrix}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{matrix}
```

要指定列格式，请使用带星号的版本和可选参数。这适用于所有其他 `matrix` 环境。

```
\begin{matrix*}[l|r]
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{matrix*}
```

#### `pmatrix`

带圆括号作为定界符的矩阵。

```
\begin{pmatrix}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{pmatrix}
```

#### `bmatrix`

带方括号作为定界符的矩阵。

```
\begin{bmatrix}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{bmatrix}
```

#### `Bmatrix`

带花括号作为定界符的矩阵。

```
\begin{Bmatrix}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{Bmatrix}
```

#### `vmatrix`

带单竖线作为定界符的矩阵。

```
\begin{vmatrix}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{vmatrix}
```

#### `Vmatrix`

带双竖线作为定界符的矩阵。

```
\begin{Vmatrix}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{Vmatrix}
```

#### `smallmatrix`

一种适合与文本在同一行显示的矩阵。

```
\begin{smallmatrix}
  a + 1 & b + 1 \\
  c     & \frac{1}{d}
\end{smallmatrix}
```

### 其他环境

#### `cases`、`dcases` 和 `rcases`

使用这些环境编写分段函数：

```
f(n) = \begin{cases}
  1          & \text{if } n = 0 \\
  f(n-1) + f(n-2) & \text{if } n \ge 2
\end{cases}
```

要使用显示样式排版内容，请改用 `dcases`：

```
f(n) = \begin{dcases}
  1          & \text{if } n = 0 \\
  f(n-1) + f(n-2) & \text{if } n \ge 2
\end{dcases}
```

要将花括号显示在右侧，请使用 `rcases`：

```
f(n) = \begin{rcases}
  1          & \text{if } n = 0 \\
  f(n-1) + f(n-2) & \text{if } n \ge 2
\end{rcases}
```

#### `gather`

连续的多行方程，无对齐。

```
\begin{gather}
  3(a-x) = 3.5x + a - 1 \\
  3a - 3x = 3.5x + a - 1 \\
  a = \frac{13}{4} , x - \frac{1}{2}
\end{gather}
```

#### `multline`

第一行左对齐，最后一行右对齐，中间行居中对齐。

```
\begin{multline}
  3(a-x) = 3.5x + a - 1 \\
  3a - 3x = 3.5x + a - 1 \\
  a = \frac{13}{4}x - \frac{1}{2}
\end{multline}
```

#### `align`

```
\begin{align}
  f(x) & = (a+b)^2 \\
       & = a^2+2ab+b^2
\end{align}
```

#### 其他

- `\begin{math} x+\frac12 \end{math}` — 行内数学环境
- `\begin{displaymath} x+\frac12 \end{displaymath}` — 显示数学环境
- `\begin{equation} x+\frac12 \end{equation}` — 带编号的公式
- `\begin{subequations} x+\frac12 \end{subequations}` — 子公式
- `\begin{eqnarray} x+\frac12 \end{eqnarray}` — 等号对齐

避免使用 `center`，改用 `align`。

```
\begin{center}
  \text{first}
\end{center}
```

以下环境本身不构成数学环境，但可以作为更复杂结构的构建块：

- `\begin{gathered}...\end{gathered}`
- `\begin{split}...\end{split}`
- `\begin{aligned}...\end{aligned}`

---

## TeX 寄存器

数学排版受一些存储在"寄存器"中的"常量"影响。这些寄存器可以通过 mathfield 的 `mf.registers` 属性进行全局设置。

| 寄存器 | 说明 |
|--------|------|
| `arrayrulewidth` | array 环境中分隔线的宽度 |
| `arraycolsep` | 分隔线之间的间距 |
| `arraystretch` | 环境中行之间的拉伸因子 |
| `delimitershortfall` | 定界符短缺量 |
| `doublerulesep` | 相邻分隔线之间的间距 |
| `jot` | 所有允许多行的数学表达式中行之间的垂直间距 |
| `fboxrule` | `\boxed` 或 `\fbox` 等命令边框的默认宽度 |
| `fboxsep` | 方框与其内容之间的默认内边距 |
| `medmuskip` | 二元运算符周围的间距。另请参阅 `thinmuskip`、`thickmuskip` |
| `nulldelimiterspace` | 空定界符的水平间距 |
| `thickmuskip` | 关系运算符周围的间距。另请参阅 `medmuskip`、`thinmuskip` |
| `thinmuskip` | 数学标点符号周围的间距。另请参阅 `medmuskip`、`thickmuskip` |

---

## TeX 原语

以下命令是 TeX 原语。大多数仅在编写 TeX 宏包或宏时有用。

| 命令 | 说明 |
|------|------|
| `%` | `%` 字符后的内容（直到行尾）被解释为注释并被忽略 |
| `\limits` `\nolimits` | 控制上下限位置 |
| `\relax` | 空操作 |
| `\noexpand` | 阻止下一个标记被展开 |
| `\obeyspaces` | 在数学模式中，空格通常被忽略。使用此命令后，即使在数学模式中也会保留空格 |
| `\bgroup` `\egroup` | 开始/结束分组，等同于开/闭花括号 |
| `\string` | 将下一个标记转换为字符串 |
| `\csname` `\endcsname` | 将直到 `\endcsname` 的下一个标记转换为命令 |
| `\ensuremath{}` | 如果在数学模式中，则不执行任何操作。否则，切换到数学模式 |

---

> MathField 0.110.0
>
> Compute Engine 0.66.0
