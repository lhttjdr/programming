+++
title="2 \u0024\u005clambda\u0024演算"
weight = 2
mathjax=true
+++


## 无类型$\lambda$演算

### $\lambda$表达式

无歧义地定义一系列元素，构成可数集合，记$\mathit{VAR}$。对任意的$x \in \mathit{VAR}$，$\lambda$表达式$M$可以递归地定义如下：

- (变量, variable) 变量$x$为一个$\lambda$**表达式**。
- (函数应用, application) 若$M$和$N$为$\lambda$表达式，则$(M\ N)$为$\lambda$表达式。
- (函数抽象, abstraction) 若$x$为变量，$M$为lambda表达式，则$(\lambda x.M)$为$\lambda$表达式。
 
从$\mathit{VAR}$产生的所有的$\lambda$表达式构成的集合是一种**形式语言**，记为$\Lambda$。


使用BNF范式，可写成
$$
\def\wideOr{\mathrel{\\,\\,|\\,}}
\begin{array}{lcll}
M & ::= & x & \text{(变量, variable)} \\\\
& \wideOr & (\lambda x.M) & \text{(抽象, abstraction)} \\\\
& \wideOr & (M\ M) & \text{(应用, application)}
\end{array} $$

$\lambda$表达式的各部分名称如下:

$$ \displaystyle{
    (\overbrace{(\lambda \underbrace{w}\_\text{形式参数, parameter}.\underbrace{(w\ w)}\_\text{函数体,function body})}^\text{函数, function} \overbrace{(\lambda o.(o\ o))}^\text{实际参数, argument})}
$$

通过如下语法约定，可以减少括号的使用
$$
\begin{eqnarray}
\lambda x_1 x_2 x_3\cdots x_n . M & \equiv & (\lambda x_1.(\lambda x_2.(\lambda x_3.(\cdots (\lambda x_n.M)\cdots)))) \nonumber \\\\
M_1\ M_2\ M_3 \cdots\ M_n & \equiv & (\cdots ((M_1\ M_2)\ M_3) \cdots\ M_n) \nonumber \\\\
\lambda x.M_1\ M_2\ M_3 \cdots\ M_n &\equiv & (\lambda x.(M_1\ M_2\ M_3 \cdots\ M_n)) \nonumber
\end{eqnarray}
$$
前两条描述的是结合性：函数抽象是右结合的（right-associative），函数应用是左结合的（left-associative）。最后一条描述的是优先级：函数应用的优先级高于函数抽象。
       
       
### 概念

#### 自由变量与约束变量

一个$\lambda$表达式的自由变量(free variable)由映射$FV : \Lambda \to \{ \mathit{VAR} \}$来定义。
$$
\begin{array}{rcl}
FV(x) & = & \\{x\\} \\\\
FV(M\ N) &=& FV(M) \cup FV(N) \\\\
FV(\lambda x.M) &=& FV(M) \setminus \\{x\\} \\\\
\end{array}
$$
若变量出现在$\lambda$表达式中，且不是自由变量，则称为约束变量(bounded variable)。形式化定义如下,
$$
\begin{array}{rcl}
BV(y) & = & \\{\\} \\\\
BV(M\ N) &=& BV(M) \cup BV(N) \\\\
BV(\lambda y.M) &=& BV(M) \cup \\{y\\} \\\\
\end{array}
$$


#### 新鲜变量

既不是自由变量也不是约束变量的称为新鲜变量(fresh variable). 对于$\lambda$表达式$M$, $Fresh(M)$表示了全集$\mathit{VAR}$中$FV(M)\cup BV(M)$的补集。
$$\mathit{Fresh}(M) = \\{x\in \mathit{VAR} | x\notin FV(M) \cup BV(M) \\}$$


#### 组合子

若$\lambda$表达式$M$满足$FV(M)=\\{\\}$，则称$M$为组合子（combinator）。


### 变量替换

变量替换（substitution）是$\lambda$的基本操作。

以$N$替换$M$中的变量$x$,其结果记为$M[x:=N]$,也写作$[x/N]M$.
$$
\begin{array}{rcl}
y[x:=N] & \equiv & \begin{cases}
N\hphantom{NNNNNNNN}\hphantom{\lambda y.M} & \text{当} x=y \\\\
y & \text{其它}
\end{cases} \\\\
(M_1\ M_2)[x:=N] & \equiv & (M_1[x:=N])(M_2[x:=N]) \\\\
(\lambda y.M)[x:=N] & \equiv & \begin{cases}
\lambda y.M \hphantom{NNNNNNNN}\hphantom{N} & \text{当} x=y \\\\
\lambda y.M[x:=N] & \text{当} x\neq y \land (x\notin FV(M) \lor y\notin FV(N)) \\\\
\lambda z.M[y:=z][x:=N] & \text{其它; 其中}z\text{为第一个满足条件}z \notin \\{x\\} \cup FV(M) \cup FV(N)\text{的变量} \\\\
\end{cases}
\end{array}
$$

也可以展开来写

$$
\begin{array}{rcll}
x[x:=N] & \equiv & N & \\\\
y[x:=N] & \equiv & y &\text{其中}\ x\neq y \\\\
(M_1\ M_2)[x:=N] & \equiv & (M_1[x:=N])(M_2[x:=N]) & \\\\
(\lambda x.M)[x:=N] & \equiv & (\lambda x.M) & \\\\
(\lambda y.M)[x:=N] &\equiv& \lambda y.(M[x:=N]) & \text{其中} x\neq y,\text{且} x \notin FV(M) \lor y\notin FV(N) \\\\
(\lambda y.M)[x:=N] &\equiv& \lambda z.(M[y:=z][x:=N]) & \text{其中} x\neq y,\text{且} x \in FV(M) \land y\in FV(N)\text{，} \\\\
& & & z\text{为满足} y_n\notin FV(M) \land y_n\notin FV(N) \text{的，最小的}n\text{对应的}y_n
\end{array}
$$

## 运算

### $\alpha$变换

$\alpha$变换是约束变量的重命名。例如，下面的两个函数是等价的。形式参数$x$换成$y$是不影响函数的。

{{< tabs JavaScript Racket >}}
{{< codetab >}}
function(x) { return x + 1; }
function(y) { return y + 1; }
{{< /codetab >}}

{{< codetab >}}
(lambda (x) (+ x 1))
(lambda (y) (+ y 1))
{{< /codetab >}}

{{< /tabs >}}


$\alpha$等价($\lambda$-equivalent)表示在$\alpha$变换下保持等价的$\lambda$表达式。

$$\dfrac{}{x =\_{\alpha} x} $$

$$\dfrac{M_1 =\_{\alpha} M_2 \qquad N_1 =\_{\alpha}N_2}{M_1\ M_2 =\_{\alpha} N_1\ N_2}$$

$$\dfrac{M_1[x:=z] =\_{\alpha} M_2[y:=z] \qquad z\notin FV(M_1)\cup FV(M_2) }{\lambda x.M_1=\_{\alpha} \lambda y.M_2}$$


### $\beta$归约

$\beta$归约描述的是实参替换形参的过程，即函数应用的展开运算。

$$(\lambda x.M)\ N \to\_{\beta} M[x := N]$$

具有左侧部分的形式的表达式可称为$\beta$**可归约式**（$\beta$ redex, $\beta$ reducible expression）。一个$\lambda$表达式中，可能出现多个$\beta$可归约式。按照下面的规则可以进行非确定的完全$\beta$归约。（非确定指同时多个可归约式时，归约的顺序不确定）

$$\dfrac{}{(\lambda x.M)\ N \to\_{\beta} M[x := N]}\qquad\qquad \dfrac{M_1\to\_{\beta}M_2}{M_1\ N\to\_{\beta}M_2\ N}$$

$$\dfrac{M_1\to\_{\beta}M_2}{\lambda x.M_1\to\_{\beta}\lambda x.M_2}\qquad\qquad\qquad\qquad\dfrac{N_1\to\_{\beta}N_2}{M\ N_1\to\_{\beta}M\ N_2}$$

- 一个$\beta$可归约式，显然地直接进行归约操作。（左上）
- 应用中的函数部分或实参部分出现$\beta$可归约式，则分别进行归约操作后再应用。（右）
- 抽象中的函数体出现$\beta$可归约式，可以先对函数体归约操作。（左下）

不含$\beta$可归约式的$\lambda$项称为$\beta$**范式**(normal form)。通过以上规则，反复进行$\beta$归约后，可以得到$\beta$范式。但是，并不是所有的$\lambda$表达式都能归约到$\beta$范式。例如：
$$(\lambda x.x\ x)\ (\lambda x. x\_ x) \to\_{\beta} (\lambda x.x\ x)\ (\lambda x. x\_ x)$$
这个式子会$\beta$归约到自身。它永远无法通过$\beta$归约去掉所有的$\beta$可归约式。

Church-Rosser定理表明，如果一个$\lambda$式存在$\beta$范式，则在$\alpha$等价的意义下，$\beta$范式是唯一的。不过值得注意的是，即便$\beta$范式存在，并不意味着按任意顺序归约都能得到$\beta$范式。例如：

$$
\begin{array}{rclcl}
    (\lambda x.\lambda y.x)\ a\ ((\lambda x.x\ x)\ (\lambda x.x\ x)) &\to\_{\beta} &  ([a/x] (\lambda y.x))\ ((\lambda x.x\ x)\ (\lambda x.x\ x)) & \equiv & ((\lambda y.a))\ ((\lambda x.x\ x)\ (\lambda x.x\ x)) \\\\
     &\to\_{\beta} &  ([((\lambda x.x\ x)\ (\lambda x.x\ x))/y]a)\  & \equiv & a
\end{array}
$$

以上经过两次$\beta$归约，可以得到$\beta$范式为$a$。如果执意先归约$((\lambda x.x\ x)\ (\lambda x.x\ x))$的部分，那便永远得不到$\beta$范式了。可见不恰当的归约顺序会导致陷入循环，无法得到$\beta$范式。


### $\eta$变换

$\eta$归约是$\lambda$演算中外延性的描述。

{{< panel title="外延性（extensionality）" type="success" >}}
两个对象相等，当且仅当它们的属性都相等。

- 两个集合相等，当且仅当它们的元素相同。（外延公理）
  - 形式定义：$\forall P\forall Q[\forall X(X\in P\leftrightarrow X\in Q) \to P=Q]$
- 两个函数相等，当且仅当对任意给定的参数，函数值对应相等。（注意，这里并没有比较函数内部的逻辑，而仅仅从外面的输入/输出来判断）
  - $\forall f\forall g[\forall x(f(x)=g(x))\to f=g]$
- 两个$\lambda$表达式相等，当且仅当应用到任意实参$x$时，两者相等。
  - $\forall M\forall N[\forall x\notin\mathit{FV}(M)\cup\mathit{FV}(N)\ (\lambda x.M\ x = \lambda x.N\ x)\to M=N]$
{{< /panel >}}

$$\dfrac{x\notin \mathit{FV}(M)}{\lambda x.M\ x \to\_{\eta} M}$$

($\eta$变换$\to$外延性) 若$f = g$，$x\notin\mathit{FV}(f)\cup\mathit{FV}(g)$，由$\eta$变换，$\lambda x.f\ x = \lambda x.g\ x$成立。

(外延性$\to\eta$变换) 若$(\lambda x. f\ x)\ y = f\ y$对任意的$y$成立。由外延性，$\lambda x.f\ x = f$成立。

## 归约策略

当一个$\lambda$式中有多个可归约项时，归约运算的顺序便是归约策略（reduction strategies）。

### 正则序

### 按名调用

### 应用序

### 按值调用


## 实例

### 邱奇编码

### 递归

## 带类型$\lambda$演算


## $\lambda$演算计算器


{{< alert style="info" >}} 以下出自马萨里克大学（Masaryk University）的[JAKUB KADLECAJ的本科学位论文](https://is.muni.cz/th/phetv/bp_kadlecaj.pdf)，开源代码 https://gitlab.com/kdlcj/lambda，遵循[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.zh)协议。为适合嵌入网页中动作行为，已经修改了某些跳转链接的target。 {{< /alert >}}


<div style="position: relative; width: 100%; overflow: hidden; padding-top: 61%;">
<iframe id="iframe" style="position: absolute; top: 0; left: 0; bottom: 0; right: 0; width: 100%; height: 100%; border: none;" src="/app/lambda/index.html" class="iframe" frameborder="0" ></iframe>
</div>