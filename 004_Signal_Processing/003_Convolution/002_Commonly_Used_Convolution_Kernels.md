- [模糊型卷积核函数](#模糊型卷积核函数)
  - [均值模糊型](#均值模糊型)
  - [高斯模糊型](#高斯模糊型)
- [增强型卷积核函数](#增强型卷积核函数)
  - [锐化型](#锐化型)
- [特征提取型卷积核函数](#特征提取型卷积核函数)
  - [梯度特征型](#梯度特征型)


> 首先声明一点，我没有做过除了图形、图像、系统以外的其他领域，所以我无法给出在其他领域里出现的卷积核函数，以及解释在其他领域中的卷积具体有什么作用。而卷积本身，我所听说过的，就有非常多不同种类，在这篇文章里我所能做的，是尽可能罗列我所知道的，或者通过一些文章找到的卷积核。至于其他的卷积核，当你不知道它能产生什么效果时，你可以在理解这些文章后，自己动手实践一下。

以下类型分类，我是按照我自己对于卷积效果的理解进行分类的，不一定是科学的严谨分类，你且当作参考。

# 模糊型卷积核函数
这一类型的卷积核函数，通常没有大小的固定情况。通常使用 $3 \times 3$，偶尔也可以见到 $5 \times 5$ 甚至更大的尺寸。

## 均值模糊型
这一类的卷积核函数，每一个元素值通常为 $1/N$。所以，对于 $3 \times 3$ 的结构来说，其每一个元素值如下：

$$
\begin{bmatrix}
1/9 & 1/9 & 1/9  \\ 
1/9  & 1/9  & 1/9 \\ 
1/9  & 1/9  & 1/9 
\end{bmatrix}
$$

其运行结果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311003259530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)


## 高斯模糊型
高斯模糊型，通常会产生模糊小细节，保留大细节的效果。也就是使用单反相机能拍出的效果。其每个元素值，通常由高斯分布的 $\sigma$ 给出，在某些程序中有时也会用二项分布值来近似逼近（后者计算速度通常更快一些）。

比如这种：

$$
\begin{bmatrix}
0.095 & 0.118  & 0.095 \\ 
0.118  & 0.148 & 0.118 \\ 
0.095  & 0.118 & 0.095 
\end{bmatrix}
$$

其运行结果如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311003313451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)


# 增强型卷积核函数

## 锐化型
$$
\begin{bmatrix}
-1 &  -1 & -1 \\ 
-1  & 9 & -1 \\ 
-1  & -1 & -1 
\end{bmatrix}
$$

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031100354811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)


# 特征提取型卷积核函数
## 梯度特征型

采用梯度公式，可以按照中心展开，向前展开，或向后展开。在矩阵中可以设定不同的方向，因此还可以产生出竖直梯度特征，水平梯度特征等不同特征的效果。

中心展开型

$$
\begin{bmatrix}
0 &  -1 & 0 \\ 
-1  &0 & 1 \\ 
0  & 1 & 0 
\end{bmatrix}
$$

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311003854552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
水平型

$$
\begin{bmatrix}
-1 &  -1 & -1 \\ 
0  &0 & 0 \\ 
1  & 1 & 1 
\end{bmatrix}
$$

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311004620314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)



垂直型

$$
\begin{bmatrix}
-1 &  0 & 1 \\ 
-1  & 0 & 1 \\ 
-1  & 0 & 1 
\end{bmatrix}
$$

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311004632583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
拉普拉斯型

$$
\begin{bmatrix}
0 &  1 & 0 \\ 
1  & -4 & 1 \\ 
0  & 1 & 0 
\end{bmatrix}
$$

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210311005143862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)

-----

关于如何手写卷积公式，请参考我的另一篇文章[《卷积——基本概念》](https://blog.csdn.net/poisonchry/article/details/114414147?spm=1001.2014.3001.5501)