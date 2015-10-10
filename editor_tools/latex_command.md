 LaTeX集合运算相关命令

需要的包
usepackage{amsmath,amssymb}

集合的大括号：                  \{ ...   }\

集合中的“|”：                \mid

属于：                    \in

不属于：                    \not\in

A包含于B：                A\subset B

A真包含于B：                A\subsetneqq B

A包含B：                    A\supset B

A真包含B：                A\supsetneqq B

A不包含于B：                A\not\subset B

A交B：                    A\cap B

A并B：                    A\cup B

A的闭包：                    \overline{A}

A减去B:                    A\setminus B

实数集合：                \mathbb{R}

空集：                    \emptyset



Markdown latex 输入实例

测试，输入下面内容：
##测试##

This expression $\sqrt{3x-1}+(1+x)^2$ is an example of a $\LaTeX$ inline equation.

he Lorenz Equations:

$$
\begin{aligned}
\dot{x} & = \sigma(y-x) \\
\dot{y} & = \rho x - y - xz \\
\dot{z} & = -\beta z + xy
\end{aligned}
$$


### markdown编辑器支持基于MathJax编写LaTeX数学公式。

MathJax是一款运行在浏览器中的开源的数学符号渲染引擎，使用MathJax可以方便的在浏览器中显示数学公式，不需要使用图片。这篇文章介绍如何使用LaTeX语法编写数学公式。

标记公式
LaTeX的数学公式有两种：行内公式和块级公式。行内公式放在文中与其它文字混编，块级公式单独成行。都使用美元符号进行标记显示。

行内公式
标记方法：使用一个美元符号包围起来

$数学公式$
例子：

这是行内公式：$Gamma(n) = (n-1)!quadforall ninmathbb N$
效果：

这是行内公式：Γ(n)=(n−1)!∀n∈N

块级公式
标记方法：使用两个美元符号包围起来

$$数学公式$$
例子：

$$ x = dfrac{-b pm sqrt{b^2 - 4ac}}{2a} $$
效果：


x=−b±b2−4ac−−−−−−−√2a
上标和下标
^表示上标，_表示下标。如果上下标的内容多于一个字符，要用{}把这些内容括起来当成一个整体。上下标是可以嵌套的，也可以同时使用。

例子：

$x^{y^z}=(1+e^x)^{-2xy^w}$
效果：

xyz=(1+ex)−2xyw

另外，如果要在左右两边都有上下标，可以用sideset命令。

例子：$sideset{^1_2}{^3_4}bigotimes$

效果：12⨂34

分数表示
方法1：frac{分子}{分母}
方法2：分子 over 分母
例子：$frac{a+b}{c+d}$　或　$1 over 3$

效果： a+bc+d 　或　13

注意：对于frac的方法，如果分子分母都是单个数，那么大括号{}可以省略，如：$frac12$表示12 。

各种括号
()、[]和|可以直接表示自己，而{}本来用于分组，因此需要用{}来表示自身，也可以使用lbrace 和rbrace来表示，其它括号见下面那个表。

例子：${[z-(1+frac23x)y]div 4}$

效果： {[z−(1+23x)y]÷4}

注意原始符号并不会随着公式大小缩放。有时候我们想要括号和分隔符显示的大点，比如上面例子中希望括号能把整个分数都包住，那么可以用left和right标记，实现自适应调整。

例子：$left(1+frac23xright)$

效果：(1+23x)

left和right标记能应用的括号很多：

符号名称	LaTex代码	例子	产生的效果
小括号	( 和 )	left(xright)	(12)
中括号	[ 和 ]	left[frac12right]	[12]
大括号	{ 和 }	left{frac12right}	{12}
取绝对值	|	left|frac12right|	∣∣12∣∣
尖括号	langle 和 rangle	leftlanglefrac12rightrangle	⟨12⟩
向上取整	lceil 和 rceil	leftlceilfrac12rightrceil	⌈12⌉
向下取整	lfloor 和 rfloor	leftlfloorfrac12rightrfloor	⌊12⌋|


注意：

left和right标记必须是成对出现的，但有时候我们只用到其中一个，比如只用一个|当作分割线，这时候可以通过.来表示空的那一方，即用left.表达左边空的情况，用right.表达右边空的情况。

例子：$left. frac{du}{dx} right| _{x=0}$

效果：dudx∣∣x=0

根号表示
根号开方使用sqrt标记，语法格式如下：

sqrt[开方次数，默认为2]{开方因子}
例子：$sqrt{x^3}$　和　$sqrt[3]{frac xy}$

效果：x3−−√ 　和　xy−−√3

注意：对于非常复杂的表达式，建议使用{...}^{1/n}代替（n是开方次数）。

省略号
数学公式中常见的省略号有两种，ldots表示与文本底线对齐的省略号，cdots表示与文本中线对齐的省略号。

例子：$f(x_1,x_2,ldots,x_n) = x_1^2 + x_2^2 + cdots + x_n^2$

效果：f(x1,x2,…,xn)=x21+x22+⋯+x2n

注意：ldot和cdot可以表示与文本底线和中线对齐的单个点。

矢量表示
矢量用vect标记实现，语法格式如下：

 vec{矢量值}
例子：$vec{a} cdot vec{b}=0$

效果：a⃗ ⋅b⃗ =0

间隔空间
通常MathJax通过内部策略自己管理公式内部的空间，因此a︹︹b与a︹︹︹︹︹b（︹表示空格）都会显示为ab 。可以通过在ab间加入空格或;增加些许间隙，quad 与 qquad 会增加更大的间隙。

例子：$a;b$ 或 $aquad b$ 或 $aqquad b$

效果：ab 或 ab 或 ab

希腊字母
下面的表格用于查询和对比。

序号	大写	LaTex代码	小写	LaTex代码	中文名称
1	A	A	α	alpha	阿尔法
2	B	B	β	beta	贝塔
3	Γ	Γ	γ	gamma	伽马
4	D	D	δ	delta	德尔塔
5	E	E	ϵ	epsilon	伊普西隆
6	Z	Z	ζ	zeta	泽塔
7	H	H	η	eta	伊塔
8	Θ	Θ	θ	theta	西塔
9	I	I	ι	iota	约塔
10	K	K	κ	kappa	卡帕
11	Λ	Λ	λ	lambda	兰姆达
12	M	M	μ	mu	缪
13	N	N	ν	nu	纽
14	X	X	ξ	xi	克西
15	O	O	ο	omicron	欧米克隆
16	P	P	π	pi	派
17	R	R	ρ	rho	柔
18	Σ	Σ	σ	sigma	西格玛
19	T	T	τ	tau	陶
20	Υ	Υ	υ	upsilon	宇普西隆
21	Φ	Φ	ϕ	phi	弗爱
22	X	X	χ	chi	卡
23	Ψ	Ψ	ψ	psi	普赛
24	Ω	Ω	ω	omega	欧米伽
异体	E	E	ε	varepsilon	异体
异体	K	K	ϰ	varkappa	异体
异体	Θ	Θ	ϑ	vartheta	异体
异体	P	P	ϖ	varpi	异体
异体	R	R	ϱ	varrho	异体
异体	Σ	Σ	ς	varsigma	异体
异体	Φ	Φ	φ	varphi	异体
特殊字符
关系运算符
± ：pm 
× ：times 
÷ ：div 
∣ ：mid 
∤ ：nmid 
⋅ ⋅：cdot 
∘ ：circ 
∗ ：ast 
⨀ ：bigodot 
⨂ ：bigotimes 
⨁ ：bigoplus 
≤ ：leq 
≥ ：geq 
≠ ：neq 
≈ ：approx 
≡ ：equiv 
∑ ：sum 
∏ ：prod 
∐ ：coprod

集合运算符
∅ ：emptyset 
∈ ：in 
∉ ：notin 
⊂ ：subset 
⊃ ：supset 
⊆ ：subseteq 
⊇ ：supseteq 
⊇ ：bigcap 
⋃ ：bigcup 
⋁ ：bigvee 
⋀ ：bigwedge 
⨄ ：biguplus 
⨆ ：bigsqcup

对数运算符
log ：log 
lg ：lg 
ln ：ln

三角运算符
⊥ ：bot 
∠ ：angle 
30∘ ：30^circ 
sin ：sin 
cos ：cos 
tan ：tan 
cot ：cot 
sec ：sec 
csc ：csc

微积分运算符
′ ：prime 
∫ ：int 
∬ ：iint 
∭ ：iiint 
∬∬ ：iiiint 
∮ ：oint 
lim ：lim 
∞ ：infty 
∇ ：nabla

逻辑运算符
∵ ：because 
∴ ：therefore 
∀ ：forall 
∃ ：exists 
≠ ：not= 
≯ ：not> 
⊄ ：notsubset

戴帽符号
y^ ：hat{y} 
yˇ ：check{y} 
y˘ ：breve{y}

连线符号
a+b+c+d¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯ ：overline{a+b+c+d} 
a+b+c+d−−−−−−−−−− ：underline{a+b+c+d} 
a+b+c1.0+d2.0 ：overbrace{a+underbrace{b+c}_{1.0}+d}^{2.0}

箭头符号
↑ ：uparrow 
↓ ：downarrow 
⇑ ：Uparrow 
⇓ ：Downarrow 
→ ：rightarrow 
← ：leftarrow 
⇒ ：Rightarrow 
⇐ ：Leftarrow 
⟶ ：longrightarrow 
⟵ ：longleftarrow 
⟹ ：Longrightarrow 
⟸ ：Longleftarrow

几个例子
例子：

$sum_{i=0}^n frac{1}{i^2}$

$prod_{i=0}^n frac{1}{i^2}$

$int_0^1 x^2 {rm d}x$

$lim_{n rightarrow +infty} frac{1}{n(n+1)}$
效果：

∑ni=01i2

∏ni=01i2

∫10x2dx

limn→+∞1n(n+1)

其它特殊字符：
空格：空格 
#：# 
$：$　 
%：% 
&：& 
_：_ 
{：{ 
}：}

字体种类
公式里的字符也有字体的选择，若要对公式的某一部分字符进行字体转换，可以用如下语法格式：

{字体标记 需转换的部分字符}
其中“字体标记”可以参照下表选择合适的字体。一般情况下，公式默认为意大利体。

字体标记	字体名词	例子	例子效果	
rm	罗马体	{rm ABCDE}	ABCDE	
bf	黑体	{bf ABCDE}	ABCDE	
Bbb	黑板粗体字	{Bbb ABCDE}	ABCDE	
sl	倾斜体	{sl ABCDE}	slABCDE	
mit	数学斜体	{mit ABCDE}	ABCDE	
scr	小体大写字母	{scr ABCDE}	ABCDE	
it	意大利体	{it ABCDE}	ABCDE	
cal	花体	{cal ABCDE}	ABCDE	
sf	等线体	{sf ABCDE}	ABCDE	
tt	打字机字体	{tt ABCDE}	ABCDE	
frak	Fraktur字母（一种德国字体）	{frak ABCDE}	ABCDE