### NP 问题转化为图着色问题算法总结

---

### SAT问题转化为图着色问题

**初始化**

i.定义该问题的颜色集为$C=\{0,1,a\}$，其中假设颜色0对应False，颜色1对应True

ii.定义布尔变量：

由于布尔变量可以从$\{0,1\}$中取值，因此定义布尔变量是指与颜色a的点相连的不定点。

此外，还可以定义取值不能为True和取值不能为False的点。

iii.定义基本逻辑运算：

a.非门：$\neg a$

为了保证输出值Y与输入值A的取值相反，可以利用图着色问题的定义，将点A与点Y相邻，从而实现非门

b.与门：$a \wedge b$

分析与操作的特点，可以得到与门实现了以下功能：

​	i.如果输入全为0，输出为0

​	ii.如果输入全为1，输出为1

​	iii.如果输入不同，输出为0

c.或门：$a \vee b$

分析与操作的特点，可以得到与门实现了以下功能：

​	i.如果输入全为0，输出为0

​	ii.如果输入全为1，输出为1

​	iii.如果输入不同，输出为1


---

#### k-Clique问题

定义：有N个顶点的图$G=(V,E)$是否存在大小为k的完全子集$S$

输入：整数k、二进制邻接矩阵$A[i][j]$

转化为SAT问题：

1.变量定义：

​	i.对于$v_i\in V$，定义变量$x_i\in \{0,1\}$，如果选择$x_i$作为k团的一部分，则值为1，否则该值为0；

​	ii.如果$x_i,x_j$相邻，则变量$A[i][j]$为1，否则为0

2.问题定义：

SAT问题中CNF范式的格式为：$φ = φ_1 ∧ φ_2 ∧ ... ∧ φ_{k-1} ∧ φ_k$，因此下一步要根据K团问题中的约束条件确定$φ_i$

3.约束条件1：

如果两个点$x_i,x_j$属于子集$S$，则两点之间有边相连，因此：
$$
φ_1 = \mathop{\bigwedge}\limits_{i,j} (x_i ∧ x_j → A[i][j])=\mathop{\bigwedge}\limits_{i,j}(¬x_i ∨ ¬x_j ∨ A[i][j])
$$
4.约束条件2：

通过不断添加满足的新节点，使得完全子集的大小为$k$

i.引入变量：

为每个$x_i$定义一组一元计数器$c^i=(c_0^i,...,c_k^i),i=1,2,...,N$，$c^i$中只有1个值为1，其余值为0，通过这种方式隐式地为节点赋值，例如，如果$x_3$的值为7，则$c_7^3$为1。

将上述要求转化为CNF格式为：
$$
φ_2 = \mathop{\bigwedge}\limits_{i} ((\mathop{\bigwedge}\limits_{m \neq n} c_m^i → ¬c_n^i ) \mathop{\wedge} (\mathop{\bigvee}\limits_{m} c_m^i ))
$$
此外定义$c^0=(1,0,...,0)$，$c_k^N$=1作为初始化，因此：
$$
φ_3 =c_0^0∧ \mathop{\bigwedge}\limits_{i\textgreater 0} ¬c_i^0 ∧ c_k^N
$$
ii.如果$x_i$=1，计数器更新如下：
$$
φ_4 =\mathop{\bigwedge}\limits_{i} (x_i → \mathop{\bigwedge}\limits_{m > 0} c_m^i↔ c_{m-1}^{i-1})
$$
iii.如果$x_i$=0，计数器更新如下：
$$
φ_5 =\mathop{\bigwedge}\limits_{i} (¬x_i → \mathop{\bigwedge}\limits_{m} c_m^i↔ c_{m}^{i-1})
$$
5.总结

K团问题转化为针对CNF $φ = φ_1 ∧ φ_2 ∧ φ_3 ∧ φ_4 ∧ φ_5$的SAT问题即可

---

#### 独立集问题

定义：有N个顶点的图$G=(V,E)$是否存在大小为k的子集$S$，使得子集中不存在相邻的顶点

转化为SAT问题：

1.独立集问题转化为团问题：

​	假设S是大小为k的独立集，那么S中的任意两点u,v均不能构成图中的边。若u,v加上边，则S中任意的u,v之间都有边，则S是团。

​	所以，要求解图G中是否存在大小为k的顶点覆盖集，等价为求解G的补图中是否存在大小为k的团问题。

2.团问题转化为SAT问题：同上

---

#### 顶点覆盖问题

定义：有N个顶点的图$G=(V,E)$是否存在大小为k的子集$S$，使得图的每条边都与其中的点相连

转化为SAT问题：

1.顶点覆盖问题转化为独立集问题：

​	假设存在一个大小为k的顶点覆盖S，则G中任意一条边e，e的两个端点中至少有一个点在S中，即等价于e的两个端点中至多有一个点在V-S中。因此，V-S中任意两个点没有边，即V-S为独立集。

​	所以，要求解图G中是否存在大小为k的顶点覆盖集，等价为求解G中是否存在大小为$|V|-k$的独立集问题。

2.独立集问题转化为团问题：同上

3.团问题转化为SAT问题：同上

---

#### 无向哈密顿路径问题

定义：在给定的图$G=(V,E)$中找到一条路径，使其恰好访问每个顶点一次

转化为SAT问题：

1.变量定义：定义变量 $p_{i,j}$，表示第i个节点出现在哈密顿路径的第j个位置上。

2.约束条件：

i.一个顶点只能出现在哈密顿路径的一个位置上

ii.哈密顿路径的一个位置上只能出现唯一一个顶点

因此：
$$
\mathcal{P}(n)=\bigwedge_{j=1}^{n}\left(\bigvee_{i=1}^{n} p_{i, j} \wedge\left(\bigwedge_{k \in\{1 . . n\} \backslash\{i\}} \neg p_{k, j}\right)\right) \wedge \bigwedge_{i=1}^{n}\left(\bigvee_{j=1}^{n} p_{i, j} \wedge\left(\bigwedge_{k \in\{1 . . n\} \backslash\{j\}} \neg p_{i, k}\right)\right)
$$
iii.当第i个节点出现在哈密顿路径的第j个位置上，则位于哈密顿路径的第j+1个位置上的点一定与第i个节点相邻，因此：
$$
\mathcal{Q}(n)=\mathop{\bigwedge}\limits_{i=1}^N\mathop{\bigwedge}\limits_{j=1}^{N-1}(p_{i,j} →\bigvee\limits_{v \in (next(i))} p_{v,j+1} )
$$
3.总结：

哈密顿路径问题可以转化为对于CNF $H(n,n)=\mathcal{P}(n) \wedge \mathcal{Q}(n)$ 的SAT问题

---

#### 无向哈密顿回路问题

定义：在给定的图$G=(V,E)$中找到一条回路，除起点外恰好访问每个顶点一次

转化为SAT问题：

1.转化为哈密顿路径问题

在给定的图$G=(V,E)$中添加顶点s,w,t，对于G中的任意一个顶点u，s与u相连，u相邻的顶点与w相连，w与t相连，从而构造出图$G'$。

原问题等价于解决图$G'$是否存在一条$<s,u,v1,v2,...,vn,w,t>$的路径。

2.同上

---

#### Bounded Post correspondence problem

定义：在字母表上定义得到两个序列$W = (w_1,...,w_n), V = (v_1,...,v_n)$，对于给定的k，找到一组索引使得$w'= w_{i_1} + w_{i_2}+ w_{i_k} = v' = v_{i_1} + v_{i_2}+ v_{i_k}，k \leq |w'|=l \leq m*k$

 <img src="C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20240824215654266.png" alt="image-20240824215654266" style="zoom:49%;" />

转化为SAT问题：

1.变量定义：

i.表达式（variable := value）使用位向量进行编码

​	例如：x为位向量(x1,x2)。因此值2【$(10)_2$】在x上由【$x1∧¬x2$】编码，用（x:=2）表示。

ii.$id_j$为索引编号，用$log_2n$位编码

iii.$wi$ 与$ vi$, $i = 1..k ∗ m$代表候选的结果向量

iiii.向量$p$编码在生成结果中的位置，长度为$k+1$

2.对于构造中的第j步，考虑对A所有元素的选择后句子的变化情况：
$$
word(j, A, u, p, id) = \mathop{\bigvee}\limits_{i=1}^{|A|} ( (id_j := i)∧ \mathop{\bigvee}\limits_{p=j}^{(j-1)*m+1} ( (p_j := p)∧(p_{j+1} := p+|ai|)∧  \mathop{\bigwedge}\limits_{x=1}^{|a_i|}(u_{p+x−1} := at(a_i, x)) ))
$$
其中：$\{j, . . . , (j − 1) ∗ m + 1\}$表示该步第一个字符可能在结果中的位置。

3.保证向量的长度以及两者之间的相等性
$$
vectorsEq(l, k) = \mathop{\bigwedge}\limits_{i=1,..,l} (wi ⇔ vi) ∧ (p^w _1 := 1) ∧ (p^v_ 1 := 1) ∧ (p^w _{k+1} := l + 1) ∧ (p^v_{k+1 }:= l + 1)
$$
4.总结：

综上，构造的CNF形式为：
$$
bpcp(k, m, W, V ) = \mathop{\bigvee}\limits_{l=k}^{m*k}( vectorsEq(l, k)∧ \mathop{\bigwedge}\limits_{j=1}^{k} (word(j, W, w, p^w, id)∧word(j, V, v, p^v, id)) )
$$

---

#### String correction problem (SCP)

定义：定义在字母表下的两个句子$A=(a_1，…，a_n)$和$B=(b_1，…，b_m),n>m$，求解是否可以通过最多k次编辑操作（包括交换和删除）将A转换为B

 <img src="C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20240824232035494.png" alt="image-20240824232035494" style="zoom:60%;" />

转化为SAT问题：

1.变量定义

i.$s^i=\{s^i_j,j=1,...,n\},i=0,...,k$为初始字符串以及编辑后可能的副本

2.自上而下定义CNF为：

$$
\operatorname{escp}(k, A, B)=\bigwedge_{i=1}^{n}\left(\mathrm{~s}_{i}^{0}:=a_{i}\right) \wedge\left(\bigvee_{j=1}^{k} \bigwedge_{i=1}^{m}\left(\mathrm{~s}_{i}^{j}:=b_{i}\right)\right) \wedge \bigwedge_{j=1}^{k} \operatorname{step}(j)
$$

即定义了该算法初始为长度为n的字符串A，在每一步step均合法时，在k步内转化为长度为m的字符串B。

3.每一步操作定义为：

$$
\operatorname{step}(j)=\bigvee_{p=1 . . n-1}(\operatorname{del}(j, p) \vee \operatorname{swap}(j, p)) \vee \operatorname{del}(j, n)
$$

​	a.删除操作：

$$
\operatorname{del}(j, p)=\left(\mathbf{s}_{p}^{j-1} \neq \varepsilon\right) \wedge \bigwedge_{i=1 . . p-1}\left(\mathrm{~s}_{i}^{j} \Leftrightarrow \mathbf{s}_{i}^{j-1}\right) \wedge \bigwedge_{i=p . n-1}\left(\mathrm{~s}_{i}^{j} \Leftrightarrow \mathrm{s}_{i+1}^{j-1}\right) \wedge\left(\mathrm{s}_{n}^{j}:=\varepsilon\right)
$$

​	b.交换操作（对位置p和p+1处的两个符号的交换进行编码）：

$$
\operatorname{swap}(j, p)=\left(\mathbf{s}_{p+1}^{j-1} \neq \varepsilon\right) \wedge \bigwedge_{i=1 . . n, i \neq p, i \neq p+1}\left(\mathbf{s}_{i}^{j} \Leftrightarrow \mathbf{s}_{i}^{j-1}\right) \wedge\left(\mathbf{s}_{p}^{j} \Leftrightarrow \mathbf{s}_{p+1}^{j-1}\right) \wedge\left(\mathbf{s}_{p+1}^{j} \Leftrightarrow \mathbf{s}_{p}^{j-1}\right)
$$



---

