- [假设有一个原始信号](#假设有一个原始信号)


我们在前面的内容中已经介绍过，小波是什么，小波是如何对信号进行分解，以及小波对信号成分是如何分析的，今天在这篇文章，也是整个小波分析最后一个章节里，我们来谈谈小波最重要的应用，也就是如何使用小波函数对信号进行去噪以及去噪后如何重构去噪后的信号。

# 假设有一个原始信号

为了更好的说明Wavelet是怎么使用的，我们这里引入一个ECG信号，也就是心电信号，该信号有一个可用的样本，被收录于scipy库中，相关的调用方法如下：

```python
import numpy as np
from scipy.datasets import electrocardiogram
```

该信号时长5分钟，采样率为360Hz，所以不太满足我们的需要，所以需要对信号进行重采样，于是

```python
from scipy.signal import resample
import matplotlib.pyplot as plt
```
然后

```python
# 生成ECG信号
ecg = electrocardiogram() # ECG信号，原始长度为5分钟，采样率为360Hz
fs = 360 # 采样频率

# 重新调整采样率为1024Hz
desired_fs = 1024 # 目标采样频率
ecg = resample(ecg, int(len(ecg) * desired_fs / fs))

# 截取前5s的信号
samples_num = desired_fs * 5 # 5s的采样点数
ecg = ecg[:samples_num]
```

接下来，我们再给原始信号增加一点白噪音感染，频率再200-230Hz之间，于是

```python
# 随机生成噪音信号
noise_freq = np.random.randint(200, 231) # 随机生成噪音频率
noise_amp = 0.02 # 噪音幅值
time = np.linspace(0, 5, samples_num)
noise = noise_amp * np.sin(2 * np.pi * noise_freq * time)
ecg_with_noise = ecg + noise
```

我们再把得到的信号plot出来，看看什么样的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/8daefc5fbd1b48dc902ddfaa74ee846c.png#pic_center)


现在我们先演练一遍wavelet对原始信号的分解和重构，至于wavelet核函数，就继续使用上一章节中实现的墨西哥帽小波了，于是我们有

```python
# 生成小波函数
wavelet = wavelet_ricker(5, 5, 1.0 / desired_fs)

# 使用该小波函数对原始信号进行小波变换
coeffs = np.convolve(ecg, wavelet, mode='same')

# 再次使用该小波函数对小波变换后的信号进行小波逆变换
reconstructed_ecg = np.convolve(coeffs, wavelet, mode='same')
```

关于该参数的具体的物理意义如下：

```shell
Wavelet frequency: 5 Hz
Wavelet sampling time: 0.0009765625 s
Wavelet number of samples: 5
Wavelet sampling frequency: 1024.0 Hz
Wavelet maximum frequency: 512.0 Hz
Wavelet minimum frequency: 204.8 Hz
```

输出的图像大概这样：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2177db0d5fb7491eb9760f000caca02f.png#pic_center)

我们可以通过调整输出参数，使得输出图像尽可能接近原函数，至于幅值和相位就别讲究这么多了，说到底那不过是常数C的问题，因为对我们结果无关痛痒，所以不在这里进行详细讨论。

然后我们来执行对有噪音信号的滤波

```python
# 开始对有噪音的信号进行去噪
coeffs = np.convolve(ecg_with_noise, wavelet, mode='same')

# 将小波变换后的系数中的噪音系数置为0
coeffs[abs(coeffs) < 0.1] = 0

# 再次使用该小波函数对小波变换后的信号进行小波逆变换
reconstructed_ecg = np.convolve(coeffs, wavelet, mode='same')
```

然后来看看最后的结果吧

![在这里插入图片描述](https://img-blog.csdnimg.cn/3e45fc598b52459d9a7a4ff28b451df3.png#pic_center)
你如果有兴趣再比较一下Wavelet和FFT的计算速度，你会发现Wavelet只要把小波核生成好后，后面的计算几乎飞快，所以这也是为什么在信号学领域小波在很多方面替代了FFT。当然，话又说回来，FFT和小波都很重要，都是应该要学习和掌握的工具，并且在不同的时候，这些工具都有非常好用的地方和它的优点。

至此，从卷积 $\rightarrow$ 傅里叶 $\rightarrow$ 小波 的全部章节就此完毕，我也算在今年过年前彻底填好了这个坑。如果大家喜欢我这个系列的文章，请收藏点赞。接下来我的博客文章会把重心移到信息论、机器学习、AI等方向，并且会继续探讨在图形图像上的应用，敬请期待。
