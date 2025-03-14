- [卷积的基本概念](#卷积的基本概念)
- [卷积运算公式](#卷积运算公式)
  - [交换律](#交换律)
  - [分配律](#分配律)
  - [结合律](#结合律)
  - [数乘结合律](#数乘结合律)
- [卷积核](#卷积核)
- [代码的基本框架](#代码的基本框架)


# 卷积的基本概念

卷积，是一个强有力的数学工具，在计算机领域中有很多非常不错的运用，能产生很多意想不到的效果和输出。

数学上，其连续函数的解析式写作：

$$
F(x) = \int_{-\infty}^{\infty} f(\tau) g(x-\tau) d\tau
$$

而离散形为：

$$
F(x) = \sum_{\tau = 0}^{N} f(\tau) g(x-\tau)
$$

其本质，表示如下一个操作[^1]：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308161810672.gif#pic_center)
通常情况下，$f(\tau)$ 表示被积函数，而 $g(x-\tau)$ 表示卷积核函数。这里多说一句，之所以不使用 $f(x)$ 表示原函数而用 $f(\tau)$ ，而且强调 $f(\tau)$ 是被积函数，是因为 $f(x)$ 与 $f(\tau)$ 之间还存在着如下关系：

$$
f(x) = \int_{-\infty}^{\infty} f(\tau) d \tau
$$

这是因为这里面多了一个滑动的概念，卷积核函数的大小不一定跟原函数一致，通常情况下，在滑动计算过程中，卷积核函数一次只处理原函数的一部分。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210211111545583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
$\tau$ 在某些教科书里，通常被称为一个与时间相关的变量。但你其实只需要理解，$\tau$ 其实是 $x$ 的一个，连续地包含了一定数量元素的子集。

# 卷积运算公式
卷积运算公式最好你能了解一下，虽然说写程序可能一辈子都用不到卷积的运算公式，但没准在论文里，或者某些考试的卷子上会出现这些公式。

## 交换律
$$x(t)*h(t) = h(t)*x(t)$$

## 分配律
$$x(t)*[g(t)+h(t)] = x(t)*g(t)+x(t)*h(t)$$

## 结合律
$$[x(t)*g(t)]*h(t) = x(t)*[g(t)*h(t)]$$

## 数乘结合律
$$a[x(t)*h(t)]=[ax(t)]*h(t)=x(t)*[ah(t)]$$

# 卷积核
卷积核的选取，我个人认为是卷积这种数学工具最重要的部分。因为对于做工程，或者做其他研究，一旦遇上需要使用卷积的部分，增强或者平滑某种信号，亦或者需要从原始信号中提取某种特征，通常意味着需要使用不同的卷积核。

对于数字图像来说，卷积核通常有这些类型，比如平滑（模糊）卷积核，锐化（增强）卷积核，以及边缘卷积核等。

卷积核函数有时会被称为卷积算子，即一个函数空间到函数空间上的映射。从我个人的理解看，他们只是名称叫法不一样，就类似于一个人的昵称、姓名、绰号一类，也就是所谓函数本尊。

另外，根据这类积分函数的特点，卷积核的大小其实是没有固定的规定。对于图像来说，常用的卷积核大小为 $3 \times 3$，但根据需要你也可以定义别的大小 $4 \times 4$，$5 \times 5$。

$3 \times 3$ 之所以比较常见，这通常是差分形式所决定的。

例如，对于拉普拉斯算子来说，其 $3 \times 3$ 的数值通常如下：

$$
\begin{bmatrix}
0 & 1 & 0 \\ 
1 & -4 & 1 \\ 
0 & 1 & 0
\end{bmatrix}
$$

之所以是这些特定的数值，并非心血来潮，而是根据拉普拉斯的中间差分的差分形式计算后得出

$$\frac{\partial^2}{\partial x^2}  \approx f(x+1, y) - 2f(x, y) + f(x-1, y)$$
$$\frac{\partial^2}{\partial y^2}  \approx f(x, y+1) - 2f(x, y) + f(x, y-1)$$

即

$$\triangledown^2f(x) \approx f(x+1, y) + f(x-1, y) + f(x, y+1) + f(x, y-1) - 4f(x, y)$$

也就是说，这里的拉普拉斯算子的运算矩阵的每一个元素的数值，是其差分形式的所对应元素的系数。

因此，如果你要自行设计卷积核，那么需要根据使用卷积核函数所对应的差分形式，重新推算出新的矩阵每一项的值。

# 代码的基本框架

```python
def convolution_kernel(data):

	# 定义卷积核函数
    kernel = np.array([[0, 1, 0],
                        [1, -4, 1],
                        [0, 1, 0]])

	# 卷积核计算
	# 将数组从二维转换为一维
    kernel = kernel.flatten()
    data = data.flatten()
	
	# 将核函数反转后和原始数据进行计算
    kernel = np.flipud(kernel)
    result = kernel * data

    # 返回加和后的值，并四舍五入
    return round(np.sum(result))


def image_convolution(image):
    # 获取图片的长宽
    width, height = image.shape
    backup = np.zeros(image.shape, np.uint8)

	# 对图片逐像素点遍历
    for i in range(1, width - 1):
        for j in range(1, height - 1):
            # 从图片中取出一个小矩阵，大小跟卷积核大小一致: 3x3
            sub_img = image[i-1:i+2, j-1:j+2]
            backup[i][j] = convolution_kernel(sub_img)

    # 返回处理后的图片
    return backup
```


[^1]:《如何通俗易懂地解释卷积》 ，https://www.zhihu.com/question/22298352