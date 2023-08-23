
## 决策树
#### 什么是决策树

决策树是一种常用的基于树的机器学习方法，其主要任务是构建一棵树进行决策，以二分类为例，二分类任务构建决策树以决定某个样本是属于两个类别中的哪一个。

#### 决策树的构建过程

以二分类问题来讲。

在构建过程中，首先所有数据都在根节点，以此为起点，选择一个属性及这个属性的某个值，将所有数据分为两个部分，分别作为其左孩子和右孩子，并分别递归执行上述过程。直到子节点不可再划分。

#### 如何选择属性和属性值进行划分

比如可以计算每个属性的信息增益、信息率或者基尼指数等数据来决定哪一个属性更适合作为划分属性；至于属性值的选择，对于连续数据来说，可以选取中位数、或者不大于中位数的值。

#### 剪枝

剪枝可以应付过拟合的问题，剪枝手段分为预剪枝和后剪枝。

预剪枝是在构建树的过程中，先预估当前节点划分是否能提升决策树的泛化性能（在验证集上），再决定是否对当前节点进行划分。

后剪枝是在决策树构建完成后，自底向上依次检查非叶子节点，将其替换为叶子节点能否提升决策树的泛化性能（在验证集上）。

#### 多变量决策树（斜决策树）

一般决策树节点的划分都是基于某个属性的某个特定值，而斜决策树则是选择一个合适的线性分类器综合所有属性的值计算出一个总的值再进行划分。



## 支持向量机

#### 什么是支持向量机

支持向量机是一种二分类模型，它的基本模型是定义在特征空间上的**间隔最大**的线性分类器；且通过核技巧可以使它成为实质上的非线性分类器。

当数据线性可分时，通过硬间隔最大化，可以学习一个硬间隔SVM；当数据近似线性可分时，通过软间隔最大化，可以学习一个软间隔SVM；当数据线性不可分时，可通过核技巧和软间隔最大化，学习一个非线性SVM。

#### SVM问题

SVM学习的是一个凸二次规划问题：
```math
\begin{aligned}
    \min_{w,b} \quad  & \frac{1}{2}||\vec{w}||^2 \\
    s.t. \quad & y_{i}(\vec{w}^T\cdot \vec x_i+b)-1 \ge 0,\quad i=1,2,...,N
\end{aligned}
```
由此解出分离超平面：
```math
\vec{w^*}^T*\vec{x}+b^*=0
```
及分类决策函数：
```math
f(x) = sign(\vec{w^*}^T\cdot\vec{x}+b^*)
```

> 注：后面将$\vec{w}$和$\vec{x}$简写为$w$和$x$

#### SVM求解

应用拉格朗日对偶性，通过求解对偶问题来得到原始问题的最优解。

首先，构建拉格朗日函数，对每一个不等式引入拉格朗日乘子a，定义拉格朗日函数：

```math
\begin{aligned}
    L(w,b,a) &= \frac{1}{2}||w||^2-\sum_{i=1}^{N}a_i(y_i(w^Tx_i+b)-1) \\
   &= \frac{1}{2}||w||^2-\sum_{i=1}^{N}a_iy_i(w^Tx_i+b)+\sum_{i=1}^{N}a_i
\end{aligned}
```
根据拉格朗日对偶性，原始问题的对偶问题是极大极小值问题：
```math
\max_a \min_{w,b} L
```
那么整个计算过程就包含两个部分：先计算$\min_{w,b} L$ ；再计算$\max_a \min_{w,b} L$

##### 先计算$\min_{w,b} L$

令L对w，b的偏导数为0：
```math
\nabla_w{}L=\frac{\partial{L}}{\partial{w}}=w-\sum_{i=1}^Na_iy_ix_i=0

\nabla_b{}L=\frac{\partial{L}}{\partial{b}}=-\sum_{i=1}^Na_iy_i=0
```
得到：
```math
\begin{aligned}
    &w=\sum_{i=1}^Na_iy_ix_i\\
    &\sum_{i=1}^Na_iy_i=0
\end{aligned}
```
带入拉格朗日函数：
```math
\begin{aligned}
    L&=\frac{1}{2}||\sum_{i=1}^Na_iy_ix_i||^2-\sum_{i=1}^{N}a_iy_i((\sum_{i=1}^Na_iy_ix_i)^Tx_i+b)+\sum_{i=1}^{N}a_i \\
    &=-\frac{1}{2}\sum_{i=1}^N\sum_{j=1}^Na_ia_jy_iy_j(x_i\cdot x_j)+\sum_{i=1}^{N}a_i
\end{aligned}
```
即
```math
\min_{w,b} L=-\frac{1}{2}\sum_{i=1}^N\sum_{j=1}^Na_ia_jy_iy_j(x_i\cdot x_j)+\sum_{i=1}^{N}a_i
```

##### 计算$\min_{w,b} L$对$a$的极大值

即对偶问题：
```math
\max_a \quad \sum_{i=1}^{N}a_i-\frac{1}{2}\sum_{i=1}^N\sum_{j=1}^Na_ia_jy_iy_j(x_i\cdot x_j)

s.t. \quad \sum_{i=1}^Na_iy_i=0 \quad a_i \ge 0,\quad i=1,2,...,N
```
解出$a_i$（如SMO算法）即可求得超平面和决策分类函数：
```math
\sum_{i=1}^N a_i^* y_i (x_i,x)+b^*=0

f(x) = sign(\sum_{i=1}^N a_i^* y_i (x_i,x)+b^*)
```
##### 软间隔SVM
以上是对于硬间隔SVM的计算过程，对于软间隔SVM，其求解的问题为：
```math
\begin{aligned}
    \min_{w,b,\xi} \quad  & \frac{1}{2}||\vec{w}||^2 + C\sum_{i=i}^N \xi_i \\
    s.t. \quad & y_{i}(\vec{w}^T\cdot \vec x_i+b)-1+\xi_i \ge 0, \\
    &\xi_i\ge0, \quad i=1,2,...,N
\end{aligned}
```
求解过程则类似
##### 非线性SVM
其求解的问题为：
```math
\begin{aligned}
    \min_{w,b,\xi} \quad  & \frac{1}{2}||\vec{w}||^2 \\
    s.t. \quad & y_{i}(\vec{w}^T\cdot \phi(\vec x_i)+b) \ge 1, \quad i=1,2,...,N
\end{aligned}
```
对应的对偶问题为：
```math
\max_a \quad \sum_{i=1}^{N}a_i-\frac{1}{2}\sum_{i=1}^N\sum_{j=1}^Na_ia_jy_iy_j<\phi(x_i), \phi(x_j)>

s.t. \quad \sum_{i=1}^Na_iy_i=0 \quad a_i \ge 0,\quad i=1,2,...,N
```
其中$<\phi(x_i)\cdot \phi(x_j)>$为核函数

#### 核函数

定义
```math
K(\phi(x_i), \phi(x_j))=<\phi(x_i), \phi(x_j)>=\phi(x_i)^T\cdot \phi(x_j)
```
为核函数

> 只要一个对称函数所对应的核矩阵半正定，它就可以作为核函数使用

> 核函数表示将输入从输入空间映射到特征空间得到的特征向量之间的内积

> 核函数的线性组合仍然是核函数；核函数的直积仍然是核函数

> 常见的核函数有：线性核，多项式核，高斯核，拉普拉斯核，Sigmoid核

#### 序列最小最优化算法（SMO）

选择两个变量，将其他变量固定 --> 得到两个变量的二次规划问题，

重复（选择-更新）的过程

## 朴素贝叶斯

> 朴素贝叶斯学习到的是**生成数据**的机制，属于生成模型，利用训练数据学习P(X|Y)和P(Y)的估计，得到联合概率分布：P(X,Y)=P(y)P(X|Y)

> 期望风险最小化 --> 后验概率最大化

> 朴素：特征之间相互独立

#### 条件独立性假设

朴素贝叶斯方法的基本假设，等价于说用于分类的特征，在类别确定的情况下都是条件独立的

```math
\begin{aligned} 
P\left(X=x | Y=c_{k}\right) &=P\left(X^{(1)}=x^{(1)}, \cdots, X^{(n)}=x^{(n)} | Y=c_{k}\right) \\ &=\prod_{j=1}^{n} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right) 
\end{aligned}
```

#### 贝叶斯定理

标准公式：
```math
P(A | B)=\frac{P(A) \times P(B | A)}{P(B)}
```

- P(A|B)是已知B发生后A的条件概率，也由于得自B的取值而被称作A的后验概率。
- P(A)是A的先验概率（或边缘概率）。之所以称为"先验"是因为它不考虑任何B方面的因素。
- P(B|A)是已知A发生后B的条件概率，也由于得自A的取值而被称作B的后验概率。
- P(B)是B的先验概率或边缘概率。
- P(B|A)/P(B)有时被称作标准似然度，贝叶斯定理可表述为：后验概率 = 标准似然度*先验概率

朴素贝叶斯计算后验概率分布：
```math
P\left(Y=c_{k} | X=x\right)=\frac{P\left(X=x | Y=c_{k}\right) P\left(Y=c_{k}\right)}{\sum_{k} P\left(X=x | Y=c_{k}\right) P\left(Y=c_{k}\right)}
```
把条件独立假设带入上式，即可得到朴素贝叶斯的基本公式：

```math
y=\arg \max _{\varepsilon_{k}} P\left(Y=c_{k}\right) \prod_{j} P\left(X^{(D)}=x^{(j)} | Y=c_{k}\right)
```

#### 参数估计

显然，由上述朴素贝叶斯的基本公式可知，朴素贝叶斯的学习意味着对先验概率 $ P\left(Y=c_{k}\right) $和条件概率
```math
\prod_{j} P\left(X^{(D)}=x^{(j)} | Y=c_{k}\right)
```
的估计

- 极大似然估计
- 贝叶斯估计
