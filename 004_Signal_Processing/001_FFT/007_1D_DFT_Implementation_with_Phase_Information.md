- [引入相位的变化](#引入相位的变化)
  - [1维 DFT 实现](#1维-dft-实现)


> 这应该是本系列的最后两章，这一章里面，我将提供一个完整的DFT的实现，而在这系列的最后一章，将提供它的逆计算。如果你从这个系列的第一章追到这里，请为自己感到自豪，尽管傅里叶级数并不是很艰深的知识点，但是它所体现的数学思想，确实非常难以让刚进入大学本科学习阶段的学生难以适应。实际上我也是因为工作的原因，因为经常使用到这个工具，所以才会有了想要深入了解它的兴趣。并且每一次重新阅读相关论文和著作，都会有不一样的认识和看法。

# 引入相位的变化

## 1维 DFT 实现

上一章中，我们通过简单的用一组不同频率的余弦函数，对原信号进行了采样，并统计。并且只给出了DFT的实现代码，而且有很多限制条件。现在我们将完整的按照公式的定义，重新改写已有的 DFT 函数，并且实现 iDFT 函数，然后我们将尝试重新输入另外一组周期函数后，并在进行 DFT 之后，是否还能够转换回原始的信号。

首先，由原始信号获得它的频率组成的 DFT 函数的离散表达式是：

$$C_k = \sum f(n)cos(2\pi k n/N) - i  \sum f(n) sin(2\pi k n/N)$$

可以知道它需要存储两组不同的信息，所以我们可以先实现一个类

```python
class ComplexNumber(object):

    def __init__(self):
        self.real = 0
        self.img = 0

    def amplitude(self):
        return sqrt(self.real ** 2 + self.img ** 2)
```

生成原始信号的代码，在原来的基础上改动一下就好
```python
def generate_signals(pts: int):
    x_axis = np.linspace(-np.pi, np.pi, pts)  # 假设 1s 内完成一个时间周期，即 1Hz
    return x_axis, np.sin(4 * x_axis + np.pi / 4) + 6 * np.sin(18 * x_axis)  # 根据 X 轴坐标生成 1s 内的原始信号
```
这样，尽管原始信号中包含一个4Hz，18Hz的信号，但是有相位和振幅的变化，你也可以根据自己需要任意修改。

然后dft的相关计算方法修改如下：

```python
def calculate_ck(signals, N, k):
    C_k = ComplexNumber()  # ck以复数形式进行表示
    pi = np.pi  # pi
    n = 0  # 采样点
    steps = round(len(signals) / N)  # 采样步进，默认的采样步进为1，采样方式为全尺寸采样

    while n < N:
        # 这里的 k 是采样频率
        C_k.real = C_k.real + cos(2 * pi * n * k / N) * signals[steps * n]
        C_k.imag = C_k.imag - sin(2 * pi * n * k / N) * signals[steps * n]

        n += 1

    return C_k


def dft_1d(signals, sampling_freq, dft_lens):
    dft_bins = []

    maximum_sampling_freq = sampling_freq  # 希望使用的采样最大频率
    increased_sampling_freq = maximum_sampling_freq / dft_lens  # 采样率步进
    current_sampling_freq = 0  # 当前的采样率，从 0Hz 开始进行采样

    while current_sampling_freq < maximum_sampling_freq:
        ck = calculate_ck(signals, len(signals), current_sampling_freq)
        dft_bins.append(ck)

        current_sampling_freq += increased_sampling_freq

    return dft_bins
```

输出结果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210226151419838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
然后跟标准的Numpy库的fft函数进行对比：

```python
    x_axis, signal = generate_original_signals(50)
    my_dft = dft_1d(signal, len(signal), len(signal))
    np_dft = np.fft.fft(signal, len(signal))

    for i in range(50):
        np_real = round(np_dft[i].real, 3)
        np_imag = round(np_dft[i].imag, 3)
        my_real = round(my_dft[i].real, 3)
        my_imag = round(my_dft[i].img, 3)

        var_real = (np_real - my_real) ** 2
        var_imag = (np_imag - my_imag) ** 2

        # print out
        print(f"np:\t{np_real}\t{np_imag}\t\tdft:\t{my_real}\t{my_imag}\t\tvar\t{var_real}\t{var_imag}")
```

```
np:     0.707   0.0             dft:    0.707   0.0             var     0.0     0.0
np:     0.772   -0.3            dft:    0.772   -0.3            var     0.0     0.0
np:     1.019   -0.664          dft:    1.019   -0.664          var     0.0     0.0
np:     1.808   -1.316          dft:    1.808   -1.316          var     0.0     0.0
np:     21.594  -13.053         dft:    21.594  -13.053         var     0.0     0.0
np:     -1.567  0.326           dft:    -1.567  0.326           var     0.0     0.0
np:     -0.453  -0.469          dft:    -0.453  -0.469          var     0.0     0.0
np:     0.027   -0.885          dft:    0.027   -0.885          var     0.0     0.0
np:     0.405   -1.23           dft:    0.405   -1.23           var     0.0     0.0
np:     0.792   -1.57           dft:    0.792   -1.57           var     0.0     0.0
np:     1.245   -1.936          dft:    1.245   -1.936          var     0.0     0.0
np:     1.814   -2.354          dft:    1.814   -2.354          var     0.0     0.0
np:     2.571   -2.857          dft:    2.571   -2.857          var     0.0     0.0
np:     3.629   -3.5            dft:    3.629   -3.5            var     0.0     0.0
np:     5.208   -4.38           dft:    5.208   -4.38           var     0.0     0.0
np:     7.783   -5.711          dft:    7.783   -5.711          var     0.0     0.0
np:     12.634  -8.062          dft:    12.634  -8.062          var     0.0     0.0
np:     24.788  -13.663         dft:    24.788  -13.663         var     0.0     0.0
np:     104.066 -48.998         dft:    104.066 -48.998         var     0.0     0.0
np:     -67.954 26.882          dft:    -67.954 26.882          var     0.0     0.0
np:     -29.578 9.593           dft:    -29.578 9.593           var     0.0     0.0
np:     -20.615 5.28            dft:    -20.615 5.28            var     0.0     0.0
np:     -16.833 3.201           dft:    -16.833 3.201           var     0.0     0.0
np:     -14.942 1.881           dft:    -14.942 1.881           var     0.0     0.0
np:     -14.019 0.879           dft:    -14.019 0.879           var     0.0     0.0
np:     -13.74  0.0             dft:    -13.74  0.0             var     0.0     0.0
np:     -14.019 -0.879          dft:    -14.019 -0.879          var     0.0     0.0
np:     -14.942 -1.881          dft:    -14.942 -1.881          var     0.0     0.0
np:     -16.833 -3.201          dft:    -16.833 -3.201          var     0.0     0.0
np:     -20.615 -5.28           dft:    -20.615 -5.28           var     0.0     0.0
np:     -29.578 -9.593          dft:    -29.578 -9.593          var     0.0     0.0
np:     -67.954 -26.882         dft:    -67.954 -26.882         var     0.0     0.0
np:     104.066 48.998          dft:    104.066 48.998          var     0.0     0.0
np:     24.788  13.663          dft:    24.788  13.663          var     0.0     0.0
np:     12.634  8.062           dft:    12.634  8.062           var     0.0     0.0
np:     7.783   5.711           dft:    7.783   5.711           var     0.0     0.0
np:     5.208   4.38            dft:    5.208   4.38            var     0.0     0.0
np:     3.629   3.5             dft:    3.629   3.5             var     0.0     0.0
np:     2.571   2.857           dft:    2.571   2.857           var     0.0     0.0
np:     1.814   2.354           dft:    1.814   2.354           var     0.0     0.0
np:     1.245   1.936           dft:    1.245   1.936           var     0.0     0.0
np:     0.792   1.57            dft:    0.792   1.57            var     0.0     0.0
np:     0.405   1.23            dft:    0.405   1.23            var     0.0     0.0
np:     0.027   0.885           dft:    0.027   0.885           var     0.0     0.0
np:     -0.453  0.469           dft:    -0.453  0.469           var     0.0     0.0
np:     -1.567  -0.326          dft:    -1.567  -0.326          var     0.0     0.0
np:     21.594  13.053          dft:    21.594  13.053          var     0.0     0.0
np:     1.808   1.316           dft:    1.808   1.316           var     0.0     0.0
np:     1.019   0.664           dft:    1.019   0.664           var     0.0     0.0
np:     0.772   0.3             dft:    0.772   0.3             var     0.0     0.0
```

结果是一样的，说明正确。