#为什么要引入FOLLOW集的概念？
考虑文法G[S]: 
S→aAS→aA 
S→dS→d 
A→bASA→bAS 
A→εA→ε 
求得各终结符和符号串的FIRST集合如下: 
FIRST(S)={a,d}FIRST(S)={a,d} 
FIRST(A)={b,ε}FIRST(A)={b,ε} 
FIRST(aA)={a}FIRST(aA)={a} 
FIRST(d)={d}FIRST(d)={d} 
FIRST(bAS)={b}FIRST(bAS)={b} 
FIRST(ε)={ε}FIRST(ε)={ε} 
  若输入串W=abdW=abd，则试图推导出abd串的推导过程为S⇒aA⇒abAS⇒abS⇒abdS⇒aA⇒abAS⇒abS⇒abd 
  从以上推导过程中可以看到，在第2步到第3步的推导中，即abAS⇒abSabAS⇒abS时，因为当前面临的输入符号为dd，**但是最左非终结符AA的产生式右部的开始符号集都不包含dd，但有ε**，因此对于dd的匹配自然认为只能依赖于在可能的推导过程中AA的后面的符号，所以这时候选用产生式A→εA→ε向下推导。而当前AA后面的符号为SS，SS产生式右部的开始符号集包含了dd，所以例子中可用S→dS→d推导得到匹配。 

语法树如下所示: 
--------------------- 

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20170610174401466?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1amlhbjIwMTUwODA4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

很显然，我们从以上叙述中可以得出: 
  当某一非终结符的产生式中含有空产生式时，它的非空产生式右部的开始符号集两两不相交，并与在推导过程中紧跟该非终结符右部可能出现的终结符集也不相交，则仍可构造确定的自顶向下分析。因此，引入了一个文法符号的后跟符号集合。 
引入以下FOLLOW集的概念:

对A∈VNA∈VN，有 
FOLLOW(A)={a|S⟹∗⋅⋅⋅Aa⋅⋅⋅，a∈VT}FOLLOW(A)={a|S⟹∗⋅⋅⋅Aa⋅⋅⋅，a∈VT} 
若S⟹∗⋅⋅⋅AS⟹∗⋅⋅⋅A，则规定#∈FOLLOW(A)#∈FOLLOW(A) 
这里用#作为输入串的结束符，也称为输入串括号。
因此对于每一文法符号 A∈VNA∈VN，实际上求FOLLOW(A)FOLLOW(A) 
就是考察A在产生式右端的出现情况，哪些终结符号可以跟随在A后面？

使用下列规则，直至每个FOLLOW集不再增大为止：

- 首先，设S为文法的开始符号，把{#}{#}加入FOLLOW(S)FOLLOW(S)中(这里#为句子括号)
- 若B→αAβB→αAβ是一个产生式，则 FIRST(β)−{ε}⊆FOLLOW(A)FIRST(β)−{ε}⊆FOLLOW(A)
- 如果B→αAB→αA或者B→αAβB→αAβ且β⟹∗εβ⟹∗ε，则 FOLLOW(B)⊆FOLLOW(A)FOLLOW(B)⊆FOLLOW(A) 
- 解释: 因为在推导过程中可能出现如下的句型序列:
- S⇒∗⋅⋅⋅α1Bβ1⋅⋅⋅⇒⋅⋅⋅α1αAββ1⋅⋅⋅⇒⋅⋅⋅α1αAβ1⋅⋅⋅

