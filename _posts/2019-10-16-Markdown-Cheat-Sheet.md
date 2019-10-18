---
title: MarkDown Cheat Sheet
description: MarkDown Cheat Sheet
categories:
 - Cheat Sheet
tags:
 - MarkDown
---


## 图片

居中

```html
<p align="center">
  <img width="70%" height="70%" src="https://raw.githubusercontent.com/yan-cheng/notes/master/StackEdit/pics/StackEdit.png">
</p>
```

<p align="center"><img 
width="70%" height="70%" src="https://raw.githubusercontent.com/yan-cheng/notes/master/StackEdit/pics/StackEdit.png"></p>

## 公式

### 多行格式

```tex
# 多行对齐不编号
\begin{split}
f(x) & = a_0  + \dots + a_n\\
     & = (a_0 + \dots + a_{2k}) + \\
     &\phantom{=} (a_1 + \dots + a_{2k+1})
\end{split}
```

$$
\begin{split}
f(x) & = a_0  + \dots + a_n\\
      & = (a_0 + \dots + a_{2k}) + \\
      &\phantom{=} (a_1 + \dots + a_{2k+1})
\end{split}
$$

```tex
# 多行对齐编号
\begin{align}
f(x) &= a_0 + \dots + a_n \tag{1} \\ 
     &= (a_0 + \dots + a_{2k}) + \notag \\
     &\phantom{=} (a_1 + \dots + a_{2k+1}) \tag{2}
\end{align}
```

$$
\begin{align}
f(x) &= a_0 + \dots + a_n \tag{1} \\ 
      &= (a_0 + \dots + a_{2k}) + \notag \\
      &\phantom{=} (a_1 + \dots + a_{2k+1}) \tag{2}
\end{align}
$$

```
# 带上下括号
z = \overbrace{
    \underbrace{x}_\text{real} + i
    \underbrace{y}_\text{imaginary}
    }^\text{complex number}
```

$$
z = \overbrace{
     \underbrace{x}_\text{real} + i
     \underbrace{y}_\text{imaginary}
     }^\text{complex number}
$$

### 空格

|空格类型|写法|效果|注释|
|:-:|:-:|:-:|:-:|
|指定宽度|a\hspace{5mm}b|$a\hspace{5mm}b$|-|
|两个quad空格|a \qquad b|$a \qquad b$|两个M的宽度|
|quad空格|a \quad b|$a \quad b$|一个M的宽度|
|大空格|a\ b|$a\ b$|1/3M宽度|
|中等空格|a\\;b|$a\;b$|2/7M宽|
|小空格|a\\,b|$a\,b$|1/6M宽度|
|没有空格|ab|$ab$|-|
|紧贴|a\\!b|$a\!b$|缩进1/6M宽度|

>M代表当前字体下接近字符 `M` 的宽度

### 符号

#### 分数

|$\frac { x }{ y }$|分数|
|:-:|:-:|
|$\frac { a }{ b }$|`\frac { a }{ b }`|
|${ a }/{ b }$|`{ a }/{ b }`|
|$\cfrac { a }{ b }$|`\cfrac { a }{ b }`|

####  根式

|$\sqrt [ {a} ]{ b }$|根式|
|:-:|:-:|
|$\sqrt [ a ]{ b }$|`\sqrt [ a ]{ b }`|

#### 指对数

|${ e }^{ x }$|指对数|
|:-:|:-:|
|$\sqrt [ a ]{ b }$|`\sqrt [ a ]{ b }`|
|${ a }_{ b }^{ c }$|`{ a }_{ b }^{ c }`|
|$_{ b }^{ c }{ a }$|`_{ b }^{ c }{ a }`|
|$\overset { a }{ b }$|`\overset { a }{ b }`|
|$\underset { a }{ b }$|`\underset { a }{ b }`|
|$\overset { a }{ \underset { b }{ c }}$|`\overset { a }{ \underset { b }{ c }`|
|${ _{ a }{ b }_{ c }}$|`{ _{ a }{ b }_{ c }}`|

#### 积分

|$\int {  }$|积分|
|:-:|:-:|
|$\int _{ a }^{ b }{ x }$|`\int _{ a }^{ b }{ x }`|
|$\iint _{ a }^{ b }{ x }$|`\iint _{ a }^{ b }{ x }`|
|$\iint _{ a }^{ b }{ x }$|`\iint _{ a }^{ b }{ x }`|
|$\iiint _{ a }^{ b }{ x }$|`\iiiint _{ a }^{ b }{ x }`|
|$\oint _{ a }^{ b }{ x }$|`\oint _{ a }^{ b }{ x }`|
|$\left.\frac{x} {2}\right\|_a^b$|`\left.\frac{x}{2}\right\|_a^b`|

#### 连和连积

|$\sum$|求和积操作符|
|:-:|:-:|
|$\sum _{ 1 }^{ n }{ x }$|`\sum _{ 1 }^{ n }{ x }`|
|$\prod _{ 1 }^{ n }{ x }$|`\prod _{ 1 }^{ n }{ x }`|
|$\coprod _{ 1 }^{ n }{ x }$|`\coprod _{ 1 }^{ n }{ x }`|
|$\bigcup _{ 1 }^{ n }{ x }$|`\bigcup _{ 1 }^{ n }{ x }`|
|$\bigcap _{ 1 }^{ n }{ x }$|`\bigcap _{ 1 }^{ n }{ x }`|
|$\bigvee _{ 1 }^{ n }{ x }$|`\bigvee _{ 1 }^{ n }{ x }`|
|$\bigwedge _{ 1 }^{ n }{ x }$|`\bigwedge _{ 1 }^{ n }{ x }`|

#### 上下注释符号

|$\ddot { a }$|符号|
|:-:|:-:|
|$\dot { x }$|`\dot { x }`|
|$\ddot { x }$|`\ddot { x }`|
|$\dddot { x }$|`\dddot { x }`|
|$\mathring { x }$|`\mathring { x }`|
|$\hat { x }$|`\hat { x }`|
|$\widehat { x }$|`\widehat { x }`|
|$\tilde { x }$|`\tilde { x }`|
|$\widetilde { x }$|`\widetilde { x }`|
|$\check { x }$|`\check { x }`|
|$\acute { x }$|`\acute { x }`|
|$\grave { x }$|`\grave { x }`|
|$\breve { x }$|`\breve { x }`|
|$\bar { x }$|`\bar { x }`|
|$\overline { x }$|`\overline { x }`|
|$\underline { x }$|`\underline { x }`|
|$\overbrace { x }$|`\overbrace { x }`|
|$\underbrace { x }$|`\underbrace { x }`|
|$\overbrace { x }^{ y }$|`\overbrace { x }^{ y }`|
|$\underbrace { x }_{ y }$|`\underbrace { x }_{ y }`|
|$\vec { x }$|`\vec { x }`|
|$\overleftarrow { x }$|`\overleftarrow { x }`|
|$\underleftarrow { x }$|`\underleftarrow { x }`|
|$\overrightarrow { x }$|`\overrightarrow { x }`|
|$\underrightarrow { x }$|`\underrightarrow { x }`|
|$\overleftrightarrow { x }$|`\overleftrightarrow { x }`|
|$\underleftrightarrow { x }$|`\underleftrightarrow { x }`|
|$\xleftarrow { x }$|`\xleftarrow { x }`|
|$\xleftarrow [ x ]{ y }$|`\xleftarrow [ x ]{ y }`|
|$\xrightarrow { x }$|`\xrightarrow { x }`|
|$\xrightarrow [ x ]{ y }$|`\xrightarrow [ x ]{ y }`|
|$\boxed { x }$|`\boxed { x }`|

#### 括号

|$\\{ x \\}$|括号|
|:-:|:-:|
|$\left( x \right)$|`\left( x \right)`|
|$\left[ x \right]$|`\left[ x \right]`|
|$\\{ x \\}$|`\left\{ x \right\}`|
|$\left< x \right>$|`\left< x \right>`|
|$\left\lfloor x \right\rfloor$|`\left\lfloor x \right\rfloor`|
|$\left\lceil x \right\rceil$|`\left\lceil x \right\rceil`|
|$\left\| x \right\|$|`\left\| x \right\|`|
|$\ulcorner x \urcorner$|`\ulcorner x \urcorner`|

#### 矩阵

|矩阵||
|:-:|:-:|
|$\begin{cases} f_1(x) \\ g(x) \\ h(x) \end{cases}$|`\begin{cases} f_1(x) \\ g(x) \\ h(x) \end{cases}`|
|$\begin{matrix} a & b \\ c & d \end{matrix}$|`\begin{matrix} a & b \\ c & d \end{matrix}`|
|$\begin{pmatrix} a & b \\ c & d \end{pmatrix}$|`\begin{pmatrix} a & b \\ c & d \end{pmatrix}`|
|$\begin{bmatrix} a & b \\ c & d \end{bmatrix}$|`\begin{bmatrix} a & b \\ c & d \end{pmatrix}`|
|$\begin{Bmatrix} a & b \\ c & d \end{Bmatrix}$|`\begin{Bmatrix} a & b \\ c & d \end{pmatrix}`|
|$\begin{vmatrix} a & b \\ c & d \end{vmatrix}$|`\begin{vmatrix} a & b \\ c & d \end{pmatrix}`|
|$\begin{Vmatrix} a & b \\ c & d \end{Vmatrix}$|`\begin{Vmatrix} a & b \\ c & d \end{pmatrix}`|

#### 箭头

|箭头||||||||
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|$\leftarrow$|`\leftarrow`|$\rightarrow$|`\rightarrow`|$\uparrow$|`\uparrow`|$\downarrow$|`\downarrow`|
|$\Leftarrow$|`\Leftarrow`|$\Rightarrow$|`\Rightarrow`|$\Uparrow$|`\Uparrow`|$\Downarrow$|`\Downarrow`|
|$\leftrightarrow$|`\leftrightarrow`|$\updownarrow$|`\updownarrow`|$\Leftrightarrow$|`\Leftrightarrow`|$\Updownarrow$|`\Updownarrow`|
|$\nleftarrow$|`\nleftarrow`|$\nLeftarrow$|`\nLeftarrow`|$\nleftrightarrow$|`\nleftrightarrow`|$\nLeftrightarrow$|`\nLeftrightarrow `|
|$\leftharpoonup$|`\leftharpoonup`|$\leftharpoondown$|`\leftharpoondown`|$\rightharpoonup$|`\rightharpoonup`|$\rightharpoondown$|`\rightharpoondown`|
|$\upharpoonleft$|`\upharpoonleft`|$\upharpoonright$|`\upharpoonright`|$\downharpoonleft$|`\downharpoonleft`|$\downharpoonright$|`\downharpoonright`|
|$\leftrightharpoons$|`\leftrightharpoons`|$\rightleftharpoons$|`\rightleftharpoons`|||||

#### 字体

|字体|写法|说明|使用情况|
|-|-|-|-|
|${\mathit{ABC~abc~123}}$|`{\mathit{ABC~abc~123}}`|Italicised font|Multi-letter function or variable names. Compared to \mathnormal, words are spaced more naturally and numbers are italicized as well.|
|$\mathbf{ABC~abc~123}$|`\mathbf{ABC~abc~123}`|Bold font|Vectors.|
|$\mathfrak{ABC~abc~123}$|`\mathfrak{ABC~abc~123}`|Fraktur|Almost canonical font for Lie algebras, ideals in ring theory.|
|$\mathcal{ABC}$|`\mathcal{ABC}`|Calligraphy (uppercase only)|Often used for sheaves/schemes and categories, used to denote cryptological concepts like an alphabet of definition (${\mathcal {A}}$), message space (${\mathcal {M}}$), ciphertext space (${\mathcal {C}}$), key space (${\mathcal {K}}$), Kleene's ${\mathcal {O}}$, Laplace transform (${\mathcal {L}}$).|
|$\mathbb{ABC}$|`\mathbb{ABC~abc~123}`|Blackboard bold (uppercase only)|Used to denote special sets.|
|$\mathscr{ABC}$|`\mathscr{ABC~abc~123}`|Script (uppercase only)|An alternative font for categories and sheaves.|

#### 操作符

|操作符||||||||
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|$\div$|`\div`|$\times$|`\times`|$\pm$|`\pm`|$\mp$|`\mp`|
|$\ast$|`\ast`|$\circ$|`\circ`|$\bullet$|`\bullet`|$\cdot$|`\cdot`|
|$\neq$|`\neq`|$\equiv$|`\equiv`|$\nless$|`\nless`|$\ngtr$|`\ngtr`|
|$\nleq$|`\nleq`|$\ngeq$|`\ngeq`|$\lneq$|`\lneq`|$\gneq$|`\gneq`|
|$\le$|`\le`|$\ge$|`\ge`|$\leqq$|`\leqq`|$\geqq$|`\geqq`|
|$\sim$|`\sim`|$\simeq$|`simeq`|$\approx$|`\approx`|$\cong$|`\cong`|
|$\ll$|`\ll`|$\gg$|`\gg`|$\llless$|`\llless`|$\gggtr$|`\gggtr`|

#### 集合

|集合||||||||
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|$\cap$|`\cap`|$\cup$|`\cup`|$\wedge$|`\wedge`|$\vee$|`\vee`|
|$\in$|`\in`|$\notin$|`\notin`|$\subset$|`\subset`|$\supset$|`\supset`|
|$\subseteq$|`\subseteq`|$\supseteq$|`\supseteq`|$\nsubseteq$|`\nsubseteq`|$\nsupseteq$|`\nsupseteq`|
|$\subsetneq$|`\subsetneq`|$\supsetneq$|`\supsetneq`|$\circleddash$|`\circleddash`|$\oplus$|`\oplus`|
|$\ominus$|`\ominus`|$\otimes$|`\otimes`|$\oslash$|`\oslash`|$\odot$|`\odot`|

#### 特殊符号

|特殊符号||||||||
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|$\dots$|`\dots`|$\vdots$|`\vdots`|$\cdots$|`\cdots`|$\ddots$|`\ddots`|
|$\angle$|`\angle`|$\measuredangle$|`\measuredangle`|$\sphericalangle$|`\sphericalangle`|$\bot$|`\bot`|
|$\nabla$|`\nabla`|$\sharp$|`\sharp`|$\therefore$|`\therefore`|$\because$|`\because`|
|$\neg$|`\neg`|$\parallel$|`\parallel`|$\prime$|`\prime`|$\backprime$|`\backprime `|
|$\infty$|`\infty`|$\emptyset$|`\emptyset`|

#### 希腊字母

|希腊字母||||||||
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|$\Alpha$|`\Alpha`|$\alpha$|`\alpha`|$\Beta$|`\Beta `|$\beta$|`\beta `|
 
 #### Typeface
 
|Typeface||||||||
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|$\forall$|`\forall`|$\exists$|`\exists`|$\nexists$|`\nexists`|$\partial$|`\partial`|

