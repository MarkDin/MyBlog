##### 项目

在文法产生式右部适当位置添加一个圆点构成一个项目

==空产生式,A-->ε仅有一个项目A-->●==

##### 等价项目

圆点后面是非终结符,此非终结符为左部的项目即为等价项目

##### 项目集

通常一个状态的项目集指这个状态的闭包,即包含其所有等价项目的项目的集合

**数学定义和求法为:**

1. I的项目在CLOSURE(I)中.
2. 若A-->α●Bβ属于CLOSURE(I),则每一个形如B-->●γ的项目也属于CLOSURE(I).
3. 重复步骤2知道不出现新的项目,即CLOSURE(I)不再增大.

##### 如何通过初始状态求出其他所有状态

相邻的两个状态其实出自同一个产生式,只是圆点的位置相差一,一个状态的项目集中可能有多个不同项目,并且每个项目圆点后面的符号也可能不同,将每个项目原点位置向后移动一位

##### 冲突

*在同一个状态中,存在大于一种可执行操作*

###### 移进-规约冲突

###### 规约规约冲突

##### GOTO函数

返回文法G对于符号X的后继项目闭包

#### LR(0)分析表构造算法

![1545274150233](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1545274150233.png)

#### SLR分析

follow集可以帮助我们解决LR(0)文法的冲突问题

#### LR(1)分析

仅利用follow集来解决冲突是不够的

![1545293076093](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1545293076093.png)

follow集只是可以帮我们排除一些不合理的归约,不是完全解决冲突

###### 展望符(lookahead)

#### LALR分析(lookAheadLR)

- 实质是在LR(1)基础上进行了同心项目集的合并
- 但是合并同心项目集也会产生`归约-归约`冲突
- 合并同心项目集还会推迟错误的发现,遇到某个输入本应该立即报错,但由于进行了合并,致使容错率变大,将错误推到下几步发现
- LALR会做多余的归约动作, 但绝不会做错误的移进动作,因为在合并同心项目的时候,实质是合并不同的展望符,而展望符和移进没有任何关系

#### LR文法错误处理

- 恐慌模式恢复
- 短语层次恢复

### 问题和笔记

一个文法是LR(K)的条件是当我们在一个最右句型中看到右部产生式的右部时,我们在向前看k个符号就可以决定是否使用这个产生式进行归约.这个要求比LL(k)要宽松的多.对于LL(k)文法,我们在决定是否使用某个产生式时,只能向前看这个产生式右部推导出的串的前k个符号.因此,LR文法能够比LL文法描述更多的语言



项的直观表示,就是我们已经看到了一个产生式的哪些部分.

### 编程

- 一个项可以用一对整数表示,第一个数表示产生式的编号,第二个数表示圆点的位置
- 要记录状态与状态之间进行转换的文法符号a

#### LR语法分析行为

- 移入
- 归约

归约时,先将栈顶的n个长度符号弹出栈,使得s为当前栈顶状态, A为当前输入符号不变(*归约时,输入符号不变*), 然后将GOTO(s, A)压入栈.

- 接受
- 报错