---
title: 四元素理解与应用
layout: cnmath
categories: cn
time: 2018-05-29 13:09:46
tags: [robotics, SLAM]
---

# 介绍
传说汉弥尔顿和妻子海伦在皇家爱尔兰学院一起散步，突然想到增加第四个维度来增加三元组。在这一突破的鼓舞下，当这对夫妇穿过皇家运河布鲁格姆桥时，他刻下了新发现的四元数方程[3]。

最早的时候使用欧拉角进行旋转矩阵的表达，后来使用了李群进行表示。四元素也是一种应用很广泛的旋转矩阵表达方式，在这篇博客中，我们对四元素进行旋转矩阵表达的内容。根据欧拉角以及李群的相关经验，理解四元素至少需要回答如下问题
1. 四元素本身的表示方式，以及其性质。
2. 与旋转矩阵的相互转换关系，如何实现对向量进行旋转。
3. 涉及到四元素时，如何求导，有两类情况需要处理$$R,R^T$$。
4. 如何对四元素进行更新，也就是特殊加法。


# 1. 四元素表达以及性质
四元数由四个数字组成，前三个表示虚数，一个表示旋转的幅度。

$$q = q_0 + q_1i+q_2j+q_3k\ \ \ ; \ \ \ i^2=j^2=k^2=-1$$


对比欧拉角中，每一个角度都是有一些性质的，比如2$$\pi$$的循环性，这里除了虚数或者是旋转幅度的性质外，还有哪些性质？以下列举了四元素的一些操作：
#### 1.1 四元数乘法
计算两个四元素的相乘，相乘的作用是什么？假设两个四元素$$q,r$$，其相乘的***意义***是首先旋转到$$q$$上，然后在其基础上旋转一个$$r$$(参考[What does multiplication of two quaternions give](https://math.stackexchange.com/questions/360286/what-does-multiplication-of-two-quaternions-give))。

$$q = q_0 + q_1i+q_2j+q_3k; r = r_0 + r_1i+r_2j+r_3k\\ 
n = q\times r = n_0 + n_1i+n_2j+n_3k\\
n_0 = r_0q_0-r_1q_1-r_2q_2-r_3q_3\\
n_1 = r_0q_1+r_1q_0-r_2q_3+r_3q_2\\
n_2 = r_0q_2+r_1q_3+r_2q_0-r_3q_1\\
n_3 = r_0q_3-r_1q_2+r_2q_1+r_3q_0\\ 
$$

参考[4]，可以发现yanbin使用向量的点积和差积，将上式表达为更紧凑的形式(公式3)，

#### 1.2 四元数结合
四元数的结合（conjugate）：旋转的幅度相等，虚数部分是其相反数。其作用是起到一些运算的作用，一个四元数与其conjugate的乘积是一个实数。参考[四元数结合](http://www.euclideanspace.com/maths/algebra/realNormedAlgebra/quaternions/functions/index.htm)

$$q = q_0 + q_1i+q_2j+q_3k\\
conj(q) = q' = q_0 - q_1i-q_2j-q_3k
$$

#### 1.3 其他操作
**规范四元数**，$$normalize(q) = \frac{q_0 + q_1i+q_2j+q_3k}{\sqrt{q_0^2 + q_1^2+q_2^2+q_3^2}}$$

**待增加**
# 2. 四元数与旋转矩阵之间的变换，如何实现对向量进行旋转。
旋转矩阵是优化运算过程中基本单元，任何一种表示都需要和旋转矩阵进行一一对应，这里给出四元数与旋转矩阵之间的转换关系，参考[quat2rotm](https://au.mathworks.com/help/robotics/ref/quaternion.quaternionrotmat.html)。

$$
q = q_0 + q_1i+q_2j+q_3k\\
R = \begin{bmatrix}2q_0^2-1+2q_1^2 & ...&...\\...&...&...\\...&...&2q_0^2-1+2q_3^2\end{bmatrix}
$$

其中省略号部分的项省去不写。因为是一一映射，从旋转矩阵也能解出对应的四元数数值。以上只是一种数量关系，想要搞清楚四元数是怎么实现旋转功能的，需要看youbin[4]中的$$L_q(v)=qvq^*$$，通过四元数乘法、共轭等性质，共同推导出四元数对向量的旋转功能，这里值得一提的是，三维向量可以当成是实部为0，向量部分的为该向量的四元数。他进一步给出了$$L_q(v)$$实际上就是对向量v绕着四元数轴旋转$$\theta$$角度。
# 3. 四元数的求导
在yanbin[4]p18页，对四元数的求导有比较清晰的推导。在状态向量中，保存的其实就是整个quaternion；就可以对整个quaternion进行求导，也就是四个量进行求导。与李群类似。

#### 
# 相关知识点
1. 相关术语；四元数可以分为两个部分，real part和vector part；
2. 对于四元数来说，他也是采用四个量来表示三个自由度的旋转，也是一种过参数化。这里四元数在的解决方式是保持前三个数为一个单位向量。


# 待check的点
1. 在yanbin[4]的论文中，提到了一个三维向量的叉积$$(p\times q)$$，为什么是那样计算的还没太搞懂;


# 参考
[1] Pose estimation using linearized rotations and quaternion algebra, Barfoot 2011.\\ 
[2] Apply rotation in three-dimensional space through complex vectors, Matlab documents.\\
[3] W. R. Hamilton. On quaternions; or on a new system of imagniaries in algebra. London,
Edinburgh, and Dublin Philosophical Magazine and Journal of Science, 25(3):489–495, \\
[4] quaternion, Yan-Bin Jia 2017 [downloaded]