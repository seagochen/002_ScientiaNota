- [前集回顾 (Review)](#前集回顾-review)
- [离散与采样 (Discrete Signals and Sampling)](#离散与采样-discrete-signals-and-sampling)
- [离散傅里叶 (Discrete Fourier Transform)](#离散傅里叶-discrete-fourier-transform)
- [不包含相位、振幅变化的信号 (Sine Signals)](#不包含相位振幅变化的信号-sine-signals)


> 某大佬曾经说过如下一句话

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210211113052260.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
> 因此，在这一章里我们徒手撸一个傅里叶分析的代码，用来处理一维信号数据。当然，从一维扩展至二维也是非常容易的。你完全可以通过理解了我这里的代码后，自己动手扩展至二维的应用，甚至到三维空间的应用。你也可以扩展到短时傅里叶，因为它们基本原理是一样的。

> 在前面一到五章节李，我尝试着系统性的介绍连续傅里叶的定理推导，这对于理解傅里叶是非常重要的。


* [《浅谈傅里叶1——从无穷级数到傅里叶》](https://blog.csdn.net/poisonchry/article/details/80840211)
* [《浅谈傅里叶2——从三角级数和到复平面映射及傅里叶级数的正交性探讨》](https://blog.csdn.net/poisonchry/article/details/80896471)
* [《浅谈傅里叶3——傅里叶级数的收敛性证明及对非周期函数的扩展应用》](https://blog.csdn.net/poisonchry/article/details/80914127)
* [《浅谈傅里叶4——全时傅里叶的优缺点及短时傅里叶的介绍》](https://blog.csdn.net/poisonchry/article/details/81004560)
*  [《浅谈傅里叶5——短时傅里叶的缺点》](https://blog.csdn.net/poisonchry/article/details/81029266)

# 前集回顾 (Review)
从无穷级数出发，得到最初的正弦函数和

$$f(t) = A_0 + \sum_{k=1}^n A_k \cdot sin(2\pi kt + \phi)$$

其中$A_0$你可以认为是Y轴的位移，$A_k$表示每一个正弦函数的振幅，$2\pi k$则是正弦函数的角频率，而$\phi$则是相位。然后又通过泰勒展开后，我们把以上公式通过和差化积转化为如下的公式：

$$f(t) = A_0 + \sum_{k=1}^n A_k \cdot (a_k cos 2 \pi kt + b_k sin 2 \pi kt)$$

其中$cos \phi = b_k$，$sin\phi = a_k$。接下来，又通过引入欧拉公式，将正余弦函数和表示为自然指数相关的函数和。

因为欧拉公式，

$$e^{ix} = cosx + i sinx$$

所以可以得到

$$f(t) = \sum_{k=-n}^{n} C_k e^{2\pi i k t}$$

而由于正弦函数本身具备的正交性，我们得以将其映射到复平面上。同时，我们得以推导出傅里叶级数的逆公式：

 $$C_k = \int_0^1  f(t) \cdot e^{-2 \pi i k t} dt$$

请记住这些连续公式的解析式形，我们在接下来会引入和它们相似的东西。

# 离散与采样 (Discrete Signals and Sampling)
在计算机引入工程界和科学界之前，大多数学者都采用连续函数的形式处理问题，但是对于计算机来说它无法处理连续问题。即便你看到某个近乎完美的正弦曲线，实际上也是通过一个个像素点，逐点打印到屏幕上的。

所以，对于模拟信号，我们通常会采取采样的方法，将模拟信号转换为数字信号。比如说对于原始信号$S(t)$来说，如果需要将它转换为数字信号，那么我们可以通过设置采样时间间隔为$T$，通过记录在改点的原信号振幅来完成信号采样。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225120821101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
这里我不详细展开数模转换的详细内容。如果大家有兴趣的话，我可以在其他文章中介绍这一点。

# 离散傅里叶 (Discrete Fourier Transform)
离散傅里叶的计算过程，和采样过程很相似，但又有所不同。诚如我在之前的文章里介绍过的，对于一个连续的复杂周期函数，我们所关心的问题其实是这个连续周期函数中包含了哪些可以用于近似拟合的正余弦函数。

为了解决这个问题，我们重新引入在前面章节里提到过的三角函数的正交性，即如下公式推导：

$$\int_{-\pi}^{\pi} 1 \cdot 1dt = 2 \pi$$
$$\int_{-\pi}^{\pi} cos(nt) \cdot sin(mt)dt = \left\{\begin{matrix}
0, m \neq n\\ 
\pi, m = n
\end{matrix}\right.$$
$$\int_{-\pi}^{\pi} cos(nt) \cdot cos(mt)dt = \left\{\begin{matrix}
0, m \neq n\\ 
\pi, m = n
\end{matrix}\right.$$
$$\int_{-\pi}^{\pi} sin(nt) \cdot sin(mt)dt = \left\{\begin{matrix}
0, m \neq n\\ 
\pi, m = n
\end{matrix}\right.$$

当然你可以自行求证一遍，由于我们是通过采样的方法来求解频率组成，所以可以通过连续公式计算当 $f(x)g(x)$ 相乘时，在一个周期频率内，只有 $m = n$ 的项可以得以保留，且结果为 $\pi$。于是可以有：

$$C(t_k)=\int f(t_k)g(t_k) dt$$

而这个，从我个人的理解，很类似物理学里 **"共振"** 的概念。也就是越接近有效信号组成成分的频率，能够获得越大振幅。那么对于离散数学空间来说，我们就可以对上面的表达式进行重新编写:

$$C(x)=\sum_{n=0}^{N-1} f(x \cdot n/N) g(x \cdot n/N)$$

其中 $k$ 是采样点，$g(x)$ 是测试频率，$f(x)$是被测试频率。对于正弦信号来说，控制信号频率的是角频率，而我们采样的时间间隔，也可以转换为控制转角度。所以以上公式就可以改写为如下：

$$C(x)=\sum_{n=0}^{N-1} f(x \cdot n/N) cos(2 \pi k \cdot n/N) $$

$f(x)$ 还是被测试频率；$cos(2 \pi k \frac{n}{N})$ 是测试频率，其中$n/N$是通过控制转角度的采样间隔，也就是一个转动周期上的采样次数；$k$是频率调节，通过给定不同的$k$，输出不同的频率；而$2\pi$ 就不用说了，就是一个转动周期。

然后我们来回顾一下傅里叶级数的逆函数：

 $$C_k = \int_0^1  f(t) \cdot e^{-2 \pi i k t} dt$$

而它对应的离散公式：

$$C_k = \sum_{n=0}^{N-1} f(n)e^{-2 \pi i k n/N}$$

然后你用欧拉公式重新展开一下，就可以得到：

$$C_k = \sum f(n)(cos2\pi k n/N - isin2\pi k n/N)$$

也就是：

$$C_k = \sum f(n)cos(2\pi k n/N) - i  \sum f(n) sin(2\pi k n/N)$$

发现没有，和我们推导出的 $C(x)=\sum_{n=0}^{N-1} f(x \cdot n/N) cos(2 \pi k \cdot n/N)$ 这个公式简直就是异父异母的亲兄弟啊。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225132447629.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
你说虚数部分？先不急，先吃七个饺子，再煮七个。


# 不包含相位、振幅变化的信号 (Sine Signals)

首先我们用这样一组正弦公式生成一段复杂的连续周期信号。

$$f(x) = sinx + sin4x + sin16x$$

它输出的图形是这样的

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225133351529.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)

那么我们怎么计算这个图像里频率的组成呢？对于计算机来说，我们可以用穷尽法来处理，也就是分别拿信号频率为

|信号频率|余弦函数|
|------------|------------|
|1Hz| $cos(x)$ |
|2Hz| $cos(2x)$ |
|3Hz| $cos(3x)$ |
|4Hz| $cos(4x)$ |
|$\cdots$| $\cdots$ |
|32Hz| $cos(32x)$ |

测试信号去测试原始的信号，然后我们看看会得到怎样的一个结果。当然，这个采样点就控制在100个上好了。

|信号频率|余弦函数|$C_k$|
|------------|------------|---------|
|0Hz| $cos(0x)$ | 0 |
|1Hz| $cos(1x)$ | 50 |
|2Hz| $cos(2x)$ | 0 |
|3Hz| $cos(3x)$ | 0 |
|4Hz| $cos(4x)$ | 50 |
|5Hz| $cos(5x)$ | 0 |
|6Hz| $cos(6x)$ | 0 |
|7Hz| $cos(7x)$ | 0 |
|8Hz| $cos(8x)$ | 0 |
|9Hz| $cos(9x)$ | 0 |
|10Hz| $cos(10x)$ | 0 |
|11Hz| $cos(11x)$ | 0 |
|12Hz| $cos(12x)$ | 0 |
|13Hz| $cos(13x)$ | 0 |
|14Hz| $cos(14x)$ | 0 |
|15Hz| $cos(15x)$ | 0 |
|16Hz| $cos(16x)$ | 50 |
|17Hz| $cos(17x)$ | 0 |
|18Hz| $cos(18x)$ | 0 |
|19Hz| $cos(19x)$ | 0 |
|20Hz| $cos(20x)$ | 0 |
|21Hz| $cos(21x)$ | 0 |
|22Hz| $cos(22x)$ | 0 |
|23Hz| $cos(23x)$ | 0 |
|24Hz| $cos(24x)$ | 0 |
|25Hz| $cos(25x)$ | 0 |
|26Hz| $cos(26x)$ | 0 |
|27Hz| $cos(27x)$ | 0 |
|28Hz| $cos(28x)$ | 0 |
|29Hz| $cos(29x)$ | 0 |
|30Hz| $cos(30x)$ | 0 |
|31Hz| $cos(31x)$ | 0 |

如果你测试的频率范围和你生成的测试数据长度一致，比方说用numpy生成这样的一个原始信号，在X轴上有一百个点，采样频率为100Hz，采样间隔为1，于是有了下面的简易DFT处理方法
```python
def generate_original_signals():
    x_axis = np.linspace(-np.pi, np.pi, 100)
    return x_axis, np.sin(x_axis) + np.sin(4 * x_axis) + np.sin(16 * x_axis)
```
分析用的DFT方法为：
```python
def correlative_sine_signals_without_phase_and_amplitude(x_axis, signals, basis):
    c_k = 0
    basis_sin = np.sin(x_axis * basis)
    steps = len(x_axis) / 100
    steps = round(steps)

    for i in range(100):
        c_k = c_k + signals[i * steps] * basis_sin[i * steps]

    return round(c_k)
```
测试用信号为：
```python
    x_axis, signals = generate_original_signals()
    for i in range(100):
        print(i, 'hz c_k is',
         correlative_sine_signals_without_phase_and_amplitude(x_axis, signals, i))
```
然后你把数据画出来，做到一张表上，就可以发现熟悉的面孔

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225212612175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
通过以上信息，我们已经可以进行对原始信号进行还原。如果相位没发生变化的情况，仅仅只是振幅变化的话，例如对于如下函数：

```python
	np.sin(1 * x_axis) + 3 * np.sin(3 * x_axis) + np.sin(7 * x_axis) + np.sin(45 * x_axis)
```

输出的图形就成了：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225234308899.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
然后，可以发现一个很有意思的信息，就是从上图可以看到3Hz的柱其高度大约是其他柱的3倍，而实际的原始函数也确实是3倍。如果原始信号没有发生相位变化的情况，使用这种办法确实能比较简单的迅速计算出频率组成。

所以，为了记录函数的相位变化信息，我们引入了虚数部分：

$$C_k = \sum f(n)cos(2\pi k n/N) - i  \sum f(n) sin(2\pi k n/N)$$

对于三角函数来说，因为它的正交性，意味着任意正弦函数实际上可以被分解为Y轴上的正弦函数分量和在X轴上的余弦函数分量。类似于在二维平面的向量，可以分解为X轴上，和Y轴上子向量之和。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225220942617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
描述一个圆周运动，往往会在很多文献中把它投影到复平面上，用螺旋线来表示这个过程。也就是为什么它在一个面上的投影是正弦，而在另外一个面上是余弦的原因。那么在下一章里，我们将继续完善这个DFT函数，并且实现它的反函数。并且测试一下当出现相位变化时，我们手撸的DFT代码能否很好的处理这些情况。

