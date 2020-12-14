+++
title = "1 图灵机"
description = ""
weight = 1
mathjax=true
+++


## 形式定义

一台图灵机是一个七元有序组$M=<Q, \Gamma, b, \Sigma, \delta, q_0, F>$，其中：

- $Q$是非空有穷**状态**(state)集合；
- $\Gamma$ 是非空有穷**带字母表**(Tape alphabet)；
- $b \in \Gamma$ 为**空白符**(blank symbol)，也是唯一允许出现无限次的字符；
- $\Sigma\subseteq \Gamma\setminus \\{b\\}$ 是非空有穷**输入字母表**(input symbol)，初始时出现在带Tape上的内容；
- $q_0 \in Q$ 是起始状态；
- $F \subseteq Q$是**终止状态**(final state)或**接受状态**(accepted state)。初始时带上的内容是被$M$接受的，当且仅当图灵机$M$最终停在接受状态。
- $\delta :(Q\setminus F) \times \Gamma \to Q\times \Gamma \times \\{L,R\\}$ 是**转移函数**(transition function)，其中$L$, $R$表示读写头是向左移还是向右移；它是一个**部分函数**(partial function)，换句话说对于某些状态$q$和字符$x$，$\delta(q,x)$可能没有定义，如果在运行中遇到没有定义的情况，机器将立刻停机。


除此外，还可显式地定义**拒绝状态**（rejected state），它是一种特殊的停机状态。原本$\delta(q,x)$未定义会造成停机，那么也可以给它一个定义，使之转移到拒绝状态而停机。在这种情况下，图灵机有三种状态：接受，拒绝，永不停机。

还有一个不常见的变种，允许转移函数中除了左右移动，还可以保持原地不动，即在上面的定义中用$\\{L,R,N\\}$代替$\\{L,R\\}$，其中$N$表示不移动(no shift, stay)。


## 基本术语


## 实例

### P'' 语言

### Brainfuck语言

下面是一个演示程序：

<div style="position: relative; width: 100%; overflow: hidden; padding-top: 115%;">
<iframe style="position: absolute; top: 0; left: 0; bottom: 0; right: 0; width: 100%; height: 100%; border: none;" src="/app/brainfuck/index.html" class="iframe" scrolling="no" frameborder="0" sandbox="allow-scripts"></iframe>
</div>

## 冯诺依曼模型