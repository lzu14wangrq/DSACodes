# Experiment 2

题解作者：LX

### 实验目的

本实验的主要目的是：
* 增进各位对栈数据结构的理解；
* 增进各位读题审题，注意细节的能力。

### 思路详解

实验背景介绍了一个故事，这么搞一下的目的主要是稍微阻挠一下那些只想着上网或者照抄课件代码的同学。至少在抄袭经典代码的时候得看懂并且修改正确，才能基本达到我们预期的要求。

所以得仔细看清这个刻意存在的障碍：运算符的优先级被修改了。

后面的要求也就只是对这一个题目的种种降低难度的限制各种情况的细节要求了，仔细阅读这些要求不难看出，需要特殊处理的“错误”只有：

1. 括号匹配
2. 除数为0
3. 0^0问题

这三个问题怎么处理呢？这三个问题恰好涉及三个不同的运算符，所以可以在处理这三个运算符的函数中处理掉，处理的方法就简单粗暴些，比如：

1. 用一个全局变量存储左括号比右括号多的数目，如果这个值<0，则括号匹配失败，表达式时错误表达式；
2. 运行除法之前事先判断一下除数是否“为0”（这里最好是更加注意下会更好，因为计算机运算的机制问题，判断一个数是否为0的依据是判断这个数是否“很小”）；
3. 和方法2差不多，判断底数和指数是否“为0”。

以上是处理特殊情况的方法，那么正常情况的处理方式有三种可以选择（对同学的编程过程体验来说是三种，对于编程出来的结果来说是两种）
1. 将网上的代码或者课件上的代码抄下来，然后找到代码中运算符优先级比较的部分，用宏定义的方式（看到这儿估计你已经懂我要干什么了）强行将运算符的那几个字符映射一下，其实也就是改变了运算符的优先级啦。或者麻烦点就是看懂别人这一块代码是怎么写的，逻辑是怎么回事儿，然后对别人的代码内部修改，以到达修改运算符优先级的目的。然而这种方法不能abs问题。但abs其实可以视为括号的同级，像括号一样参与运算，弹出右括号后取绝对值。

接下来说说正常些的方法：

2. 这是一种一边输入一边计算的方法。拿两个栈，分别是操作数栈和操作符栈，想想我们人在运算这些表达式时的思路：运算一个操作符的条件时它的下一个操作符的优先级不高于此操作符。那么我在入栈之前判断一下栈顶操作符和待入栈操作符的优先级，判断出来的结果符合刚才所说的情况就pop操作符，pop两个操作数，进行运算，push运算结果；接着判断栈顶运算符和之前那个没有入栈的运算符的优先级，如此反复进行这个过程即可。这里需要稍稍注意一下两个操作符之间得有操作数（一般来说都得考虑相关情况，然后仔细盘查特殊情况仔细查看实验要求细节，因为有负数的出现就很烦）
3. 这是一种全部信息入栈处理之后再统一进行运算的方法。整体的思路是将中缀表达式转换成逆波兰式，然后用逆波兰式的方法计算（这个该很简单了吧）。同样的两个栈，在操作符入栈的时候判断栈顶操作符和待入栈的操作符优先级，目的是将优先级高的放在栈顶，这里感觉有点像栈数据结构下的有序插入，这里得注意有“括号”时操作符的优先级会有改动，有点麻烦的是操作符之间的位置关系的变化导致操作数栈也会频繁的将操作数的位置进行改动。这个过程结束之后，接下来计算逆波兰式的过程就格外的简单了，pop操作符，pop操作数，计算，继续pop操作符，pop操作数，计算......直到栈空为止即可。

方法3很重要，逆波兰式真好用