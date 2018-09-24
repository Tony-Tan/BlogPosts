---
title: \[线性代数\]4-4:正交基和Gram算法(Orthogonal Bases and Gram-Schmidt)
categories:
  - Mathematic
  - Linear Algebra
keywords:
  - Orthogonal Matrix Q
  - Gram-Schmidt Algorithm
  - QR
toc: true
date: 2017-10-19 16:28:54
---

**Abstract:** 通过将正交的向量组合成矩阵探索其中的一些有趣的性质和用途
**Keywords:** Orthogonal Matrix Q，Gram-Schmidt Algorithm，QR

<!--more-->
# 正交基和Gram算法
刚才去了趟医院，回来的路上看到了Alpha zero的项目的一些说明，从科研或者工程角度，我们必须对这个结果进行高度的肯定，肯定什么？首先是训练时间，足够短，对于原始的神经网络一训练就是一个月半年的情况已经得到了极大的优化，第二就是训练效果，能够提高那么多也说明无论是算法结构上来说，或者训练方法上一定有很大的改进（我没看论文，只看了一些报道，这里面强化学习得到了更多的关注）；这些对于业内人士都是好消息，第一我们可以用更少的计算资源来解决更大的问题，并且更加准确。
依靠神经网络来实现人工智能，可行与否现在其实还不好说，因为我们现在只是开发出了神经网络的一些比较厉害的结果，但是对智能是否能够起到准确的模仿和实现还有待开发，连发明者们都在怀疑。观众们却被媒体忽悠的热血沸腾，到行业来做应用和研发是不明智的，一天一旦这个东西被论证为不可以的话，那岂不是又要改行，就像当年那些心怀理想加入传呼机维修，塞班应用开发的人一样，他们现在都在干什么？🐶

## 正交矩阵(Orthogonal Matrix)Q
如果我们有几个相互正交的向量，并且长度为1，把他们按列组合在一起，形成一个矩阵Q。这个矩阵就是一个正交矩阵（Orthogonal Matrix）。
这个矩阵拥有的第一个性质就是
$$
Q^TQ=I
$$

其转置和自己的乘积是单位矩阵，为什么？我们把Q展开看，如果你瞬间就明白为什么了，就跳过下面这一小段：

$$
Q=
\begin{bmatrix}
\vdots&\vdots&\vdots\\
q_1&q_2&q_3\\
\vdots&\vdots&\vdots
\end{bmatrix}\\
Q^TQ=
\begin{bmatrix}
\dots& q_1^T& \dots \\
\dots & q_2^T& \dots \\
\dots & q_3^T& \dots \\
\end{bmatrix}
\begin{bmatrix}
\vdots&\vdots&\vdots\\
q_1&q_2&q_3\\
\vdots&\vdots&\vdots\\
\end{bmatrix}=
\begin{bmatrix}
q_1^Tq_1 & q_1^Tq_2 & q_1^Tq_3 \\
q_2^Tq_1 & q_2^Tq_2 & q_2^Tq_3 \\
q_3^Tq_1 & q_3^Tq_2 & q_3^Tq_3
\end{bmatrix}=
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

证毕，如果不太明白读一下前提，$q_1,q_2,q_3$ 相互正交，其dot product是0，并且其长度都是1，所以自己和自己的dot product是1。

这个性质是比较重要和基础的，如果进行特异性，这个矩阵是方阵的时候，那么Q的逆就是他的转置，而且我们知道一个矩阵的逆既是左逆又是右逆，那么
$$
Q^TQ=I\\
QQ^T=I\\
$$
举几个Q的例子，有些我们在前面都见过，
1. 最简单的，$I=\begin{bmatrix}1&0&0\\0&1&0\\0&0&1\end{bmatrix}$ 这个家伙就是个不折不扣的Q，当然他也是I
2. 同样的由1组成的矩阵， permutation也都是Q方阵，因为我们当时在学的时候就说，P可以把一个向量变形， $P^T$ 能帮我们把他变回来（这里的P不是上一节的projection matrix是permutation matrix）。
3. Rotation: $R=\begin{bmatrix}cos\theta & -sin\theta\\ sin\theta & cos \theta\end{bmatrix}$ 旋转矩阵，也是一个Q
4. Reflection， 当 $\vec{u}$ 是单位向量的话 $Q^T=I-2\vec{u}\vec{u}^T$ 是正交矩阵，并且  $Q^T=Q^{-1}=Q$

这是几个简单的例子，Q还有一个有点就是他不会改变被乘向量的长度， $|Qx|=|x|$这是个很好的性质，如果需要迭代多次，Q能保持稳定，我之前在哪本书里见过这个说法，在某个算法里，当时觉得好奇怪，也很神奇，现在大概知道就会觉得，这个太美妙了，具体为什么不会改变，下一小节会给出完整的证明。

## 使用正交基映射(Projections Using Orthogonal Bases)
映射是上一篇（4-2）我们主要研究的内容，把结论拿出来给大家重复展示下，哒哒哒哒：
$$
\vec{p}=B\hat{\vec{x}}=\frac{BB^T}{B^TB}\vec{a}
$$
这个结果已经解释过了，但是动机要再复述一遍，我们为了把 $\vec{a}$ 映射到B span的subspace里面，也就是B的 column space，我们说到过，一个子空间是通过basis来描述的，这个B里面的每一个主列（pivot column）就是一个基，这时我们有了一个想法，就是如果我们让这些basis互相正交且长度为1，那么我们将得到一个叫Q的B（orthogonal matrix），没错，这就把前面projection的内容成功的搞到了这一篇。
$$
\vec{p}=Q\hat{\vec{x}}=\frac{QQ^T}{Q^TQ}\vec{a}\\
Q^TQ=I\\
so:\, \vec{p}=QQ^T\vec{a}=Q(Q^T\vec{a})
$$
这个看着不玄乎，我们换个角度，拆开仔细看

$$
\begin{bmatrix}
|&&|\\
q_1&\dots&q_n\\
|&&|
\end{bmatrix}
\begin{bmatrix}
q^T_1b\\
\vdots\\
q^T_nb
\end{bmatrix}=
q_1(q^T_1b)+\dots+q_1(q^T_nb)
$$
***这个看的就有点邪乎了 $q^Tb$是个标量，也就是说，如果把一个子空间用一组正交基表示，对子空间外的向量进行projection会得到一个以这组基为基的线性组合，这个重要的性质是傅里叶等数学中great transforms的基础***，卧槽，怕不怕，线性代数居然整到了傅里叶，意外不意外，惊喜不惊喜。

## Gram-schmidt
这个算法是为了给subspace换正交基的，“如果，你有了一组非正交的基，你是否也渴望有一组像隔壁老王一样的正交基了呢？告别计算烦恼，再也不用低三下四看人脸色，马上拿起电话联系Gram，不要998，只要198....”
编不下去了Gram主要是帮我做了一个算法，在已知一组非正交基的时候来找一组正交基，据Prof Strang课上讲，他也不清楚schmidt在这个算法里面做出了什么工作，但有他的名字，那么我们就当做他很重要吧，
这个算法的主要原理用到的还是projection，我们之前在projection中提到了力的分解，速度的分解，当时我就是随口一说，想起了自己的初中物理老师而已，但是这没想到在这里居然有用，假设，我们在一个三维空间里面有一个速度，我想把他分解到xy轴，这个大家都应该会，根据project to line 的方法都能得到分量，那么如果我们用原速度减去xy的分量得到的是什么？没错z轴的分量，不知道为什么？再提示你一下，你把xy分量的两个向量加起来，得到的是xy平面（子空间）的分量，对应的 $\vec{e}$ 就是z轴分量（因为Z轴是xy子空间的 orthogonal complement）：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-4-4/fenjie.png)

如果还是不太明白，没关系，往下看吧：
Gram-Schmidt算法分为以下几步，我们先假设一组向量，尾巴在一起是原点，那么这些向量都指向不同的方向，并且他们线性独立，那么我们要调整这些向量，最原始的想法就是首先，我们固定一条向量，然后调整第二个跟第一个向量垂直，然后再迭代这个过程。直到所有的向量都就位，那么这样的正交基有多少组？没错，无数组。
官方描述：

>Step 1. choosing $A=\vec{a}$

>Step 2. First Gram-Schmidt step $B=\vec{b}-\frac{A^T\vec{b}}{A^TA}A$

>Step 3. Next Gram-Schmidt step $C=\vec{c}-\frac{A^T\vec{c}}{A^TA}A-\frac{B^T\vec{c}}{B^TB}B$

>Step n. iteration

来分析下，我们之前给出的简单思路应该算是算法最初的小种子，另一种描述是，算法的核心就是把当前要处理的向量减去之前已经做好正交的那组向量（这个向量处理之前的那些向量）的分量，也就是说要求未处理的向量不能在已处理向量空间里。这就是核心思想，别问我怎么知道的，书上就这么写的，和我们的小思路也基本一致，啦啦啦，也就这样了。

## $A=QR$
矩阵分解从刚开始学到矩阵就开始讲各种各样的分解，LU，LDU这些都是以解方程组为背景的，应该属于我们的第一第二个阶段，现在我们都第三个阶段了，该说说QR分解了，那就说来话长了，那事是我刚来到深圳，前途事业一片渺茫（现在也渺茫），刚刚开始自学计算机视觉的这类知识，没有人指导，也不知道咋学，听说PCA很厉害，也不管有没有什么先验知识要学习，上来就用c语言写，查各种博客，就接触到了SVD（奇异值分解，我们后面会讲）用到了QR分解，具体的我们后面会详细讲述，但是现在回忆就是，没有人带你入门会很痛苦，你自己短时间没办法接入一个全新的或者是之前完全不了解的领域，但是一旦你自己学会了怎么入门，那么你入门其他领域应该也不会太难，弯路走了不少，但收获也会更多一些，所以我说自己是野路子，也经常告诉一些身边的小伙伴怎么快速入门，但是一旦你走了快速入门的路，你就相当于放弃了寻找入门方法的能力。
回过来继续说QR分解，上一小节介绍了Gram算法，其中有个很值得寻味的方法，就是我们只根据前面的结果和当前的输入来确定如何让这些向量正交，这就有一个很好的效果，例如
1. 我们处理第0个向量（初始那一步）这个向量的得出只和自己有关，跟后面的向量们毛关系没有。
2. 处理第一个向量，只和第0个向量和自己有关（如果不理解可以回看上面的公式）
3. 第二个向量，只和第0，1个向量以及自己有关
4. 。。。。
5. 得到完整的正交向量组

看一下数学表达:
$$
A=
\begin{bmatrix}&&\\a&b&c\\&&\end{bmatrix}=
\begin{bmatrix}&&\\q_1&q_2&q_3\\&&\end{bmatrix}
\begin{bmatrix}
q_1^Ta&q_1^Tb&q_1^Tc\\
&q_2^Tb&q_2^Tc\\
&&q_3^Tc
\end{bmatrix}
$$

说实话这个过程有点不好懂，先不说为什么会形成上三角矩阵，就说为啥要分解就不太理解，首先QR分解肯定有在实际算法中的需要，需要利用Q的某些特殊性质，而一开始并不知道分解成Q和一个矩阵的形式时，这个矩阵会是三角矩阵，但是根据我们上面的描述，以及关于上面我们关于Q的描述:

-----


$$
\begin{bmatrix}
|&&|\\
q_1&\dots&q_n\\
|&&|
\end{bmatrix}
\begin{bmatrix}
q^T_1b\\
\vdots\\
q^T_nb
\end{bmatrix}=
q_1(q^T_1b)+\dots+q_1(q^T_nb)
$$


-----
>这个看的就有点邪乎了 $q^Tb$ 是个标量，也就是说，如果把一个子空间用一组正交基表示，对子空间外的向量进行projection会得到一个以这组基为基的线性组合，这个重要的性质是傅里叶等数学中great transforms的基础


我们把上面的QR矩阵还原一下，就能得和这段话呼应的一种形式了
$$
a=q_1q_1^Ta\\
b=q_1q_1^Ta+q_2q_2^Tb\\
c=q_1q_1^Tc+q_2q_2^Tc+q_3q_3^Tc
$$
必要的说明就是,如果结合力的分解那段，或者那张图会看的非常清楚，就是把当前向量映射到一组正交基中，在处理 $\vec{a}$ 的时候正交矩阵中只有一个正交基 $q_1$,处理 $\vec{b}$ 的时候正交矩阵中只有两个个正交基 $q_1$ $q_2$ 以此类推，这也是Gram算法实现的另一种神奇的效果会产生一个上三角矩阵R
R也有一些神奇的性质，比如对角线上的元素始终都是正数，并且表示对应被分解向量的模长：
$$
A=
\begin{bmatrix}
1&2&3\\
-1&0&-3\\
0&-2&3
\end{bmatrix}
=
\begin{bmatrix}
1/\sqrt{2}&1/\sqrt{6}&1/\sqrt{3}\newline
-1/\sqrt{2}&1/\sqrt{6}&1/\sqrt{3}\newline
0&-2/\sqrt{6}&1/\sqrt{3}
\end{bmatrix}
\begin{bmatrix}
\sqrt{2}&\sqrt{2}&\sqrt{18}\\
0&\sqrt{6}&-\sqrt{6}\\
0&0&\sqrt{3}
\end{bmatrix}=QR
$$
R 的对角线元素等于A中每一个列向量的长度，神奇么?意外么？不意外，因为Q的一个性质是他不改变被乘向量的模长，前面证明过，

在实际分解中并不是使用QR分解，而是有更快的计算方法，这个我们后面肯定还要讲，因为要研究计算方法和矩阵分析，这就留个引子吧，Gram算法的具体步骤就贴个书上的吧：

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-4-4/suanfa.png)
## Conclusion
小结就是这一章的内容都好多，想写成短小精悍的都不行，因为写的少了根本解释不清楚，自己在梳理过程中也体会到了很多读书的时候没有察觉的内容，后面我们开始继续新的一章，如果有什么问题欢迎大家留言（到目前，还没有留言。。。）





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-4-4](https://www.face2ai.com/Math-Linear-Algebra-Chapter-4-4)转载请标明出处
