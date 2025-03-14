> 概率论与数理统计的相关知识，是机器学习及深度学习最常应用到的基本知识。因为对于机器学习和深度学习来说，最常见的一个应用场景就是训练一堆样本集后，给定一个测试样本，它可能同时具备类A和类B的特征，那么就需要通过概率判断它可能最终是类A，还是类B了。
>
> 这里的相关知识很多来自你大学期间学习过的教材，但也不是简单的CV大法，而是带有一些我个人的看法和我所知道的应用场景在里面，希望无论是考研还是从事相关数据分析工作都能帮到你。

# 1.1. 基本事件类型

这是我们所有概率模型的基础，所有的概率事件进行简化后都可以描述为几种以下的基本事件类型。

## 包含事件
事件发生时，若事件X属于集合A，那也一定属于B，从集合来看，A是B的子集。比如对输入字符进行检测，它可能是小写字符，也可能是数字，或者大写字符。但都属于文字字符。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705205851814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)
### 表达式
$$A \subset B$$

## 并（和）事件
事件发生时，可能属于类别A，也可能属于类别B，也可能同时属于类别A和B。在我们实际的场景中，这种情况通常出现在用户画像，一个用户可能具备多种不同的特征，比如喜欢购买烟草、啤酒、蛋白粉等，然后根据不同的特征组合判断用户潜在的消费习惯。亦或者是做聚类分析时，对于非法行为进行检测，可能非法样本具备一个或几个不同的特征，但不一定都具备所有特征集合。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705211647954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)

### 表达式

$$A \cup B = A + B$$

## 差事件
集合A包含一些集合B中存在的样本，需要计算A中不包含B的部分。比如，在实际场景中，经常见到很多集合是彼此黏着在一起的，但是某些时候，不得不完全排除其他集合，即便这些集合可能存在需要样本，但是从总体看并不影响最后的准确率，比如一些违规行为分析和判定上，存在模糊空间，有时候出于策略考虑，会放过这些模糊的行为。另外在金融安全防控中也经常这种基本模型。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070521354381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)

### 表达式
$$A - B = A \bar{B} = A - AB$$

## 交事件（积事件）
事件发生时正好出于多个集合的交集。一般较少单独拿它作为一个模型使用。但是在实际场景中也不是没有遇到过。比如说电子警察，对于交通违章行为的判定上，经常会出现判定司机驾驶行为处于违章和没有违章的情况。举例来说，前方发生车祸时，后车为了进行避让，采取了碾压实线的行为。一般来说，电子眼会把这些统统算成违章，但是最终会交给民警进行审核。如果采用自动的AI进行判定，那就会出现违章和不违章同时存在的共同事件。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705214308170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)

### 表达式
$$A\cap B = AB $$


## 互斥事件

集合A和集合B没有任何交集，也就是说从概率上A B不可能发生，要么都不发生，要么只发生其中一个，在实际应用中它跟对立事件很相似。就比如说，成绩有及格和不及格之分，性别有男性和女性之分。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705214806875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)

### 表达式

$$A \cap B = \phi$$

其中$\phi$表示空集，即概率 $P(A\cap B) = 0$

## 对立（逆）事件
有时候也叫 0-1 事件。它表示当事件发生时，它可能属于集合，也可能不属于集合。它只有二元状态，是或非。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705220115499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)

### 表达式
$$A \cup \bar{A} = S$$
$$A \cap \bar{A} = \phi$$

其中$S$表示全集，概率 $P(A) = 1 - P(\bar{A})$


## 独立事件
相对比较重要，因为很多数据模型都是基于独立事件。独立事件表示集合之间没有必然联系，没有先后顺序，可能发生也可能不发生。经典的柏松等待就是基于独立事件的，也是我们进行数据分析时经常遇到的一类问题。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705214806875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BvaXNvbmNocnk=,size_16,color_FFFFFF,t_70#pic_center)

确定一个模型或者场景是否独立事件一定基于以下几点：

* $P(AB) = P(A)P(B)$，即AB事件同时发生的概率等于单独各事件的概率乘积
* A与B之间相互独立，因此可以有 $AB$, $A \bar{B}$, $\bar{A}B$, $\bar{A}\bar{B}$
* 若A 、B、C相互独立，一定有 $P(ABC) = P(A) P(B) P(C)$
* 相互独立 $\neq$ 两两独立

这其实很好理解，就跟扔骰子一样，无论有多少个骰子，投出来的点数彼此之间都是独立的。



# 1.2. 基本事件运算规则

注意，概率的运算过程仅可以使用加减乘除，以及满足交换律，但不满足结合律，所以在运算概率的过程，请一定牢记以下公式，或者学会绘图帮助你分析。

## 德摩根律
$$P(A \cup B) = P(\bar{A} \cap \bar{B}) = P(\bar{A}\bar{B})$$

$$P(\overline{A \cap B}) = P(\bar{A} \cup \bar{B})$$

## 加法公式
$$P(A \cup B) = P(A) + P(B) - P(AB)$$

$$P(A \cup B \cup C) =P(A) + P(B) + P(C) - P(AB) - P(AC) - P(BC) + P(ABC)$$

## 减法公式
$$P(A - B) = P(A \bar{B}) = P(A - AB)$$

## 对立事件
$$P(A) = P(1 - \bar{A})$$

## 交事件/积事件

$$P(A\cap B) = P(AB) = P(A) \cdot P(B)$$

另外，就是它还有以下几种特殊的运算形式

1.  $A \cap \phi = 0$，$\phi$是空集
2. $A \cap A = A$
3. $A \cap S = A$ ， S是全集

并且由以上三个运算式，很容易扩展出其他并、差事件分别对应空集、全集的情况。

# 1.3. 基本事件练习题
我们来做点练习题，试一试你是否已经理解了这些概念

## 1.3.1 例
设A，B，C是三个事件，且$P(A) = P(B) = P(C) = 1/4$，$P(AB) = P(BC) = 0$, $P(AC) = 1/8$ 求A，B，C至少有一个发生的概率。

### 1.3.1 解
三个事件有一个发生，因此我们考虑三个事件共同包含的概率，则有

$$P(A \cup B \cup C) = P(A) + P(B) + P(C) - P(AC) - P(BC) - P(AB) + P(ABC) $$

代入概率值

$$= 5/8 + P(ABC)$$

因为从概率上讲，$P(ABC) \leq P(AB) = 0$，所以最后结果是$5/8$

### 1.3.2 例
已知 $P(A) = 1/2$ 
A，若A、B互不相容，求$P(A\bar{B})$ 
B，若$P(AB) = 1/8$，求$P(A\bar{B})$ 

### 1.3.2 解
直接按公式来

A：$P(A\bar{B}) = P(A) - P(AB) = 1/2 - 0 = 1/2$
B：$P(A\bar{B}) = P(A) - P(AB) = 1/2 - 1/8 = 3/8$ 

### 1.3.3 例
A，B是两个事件

A：已知 $A\bar{B} = \bar{A} B$，求证A=B 
B：验证事件A或B恰有一个发生的概率，等于 $P(A) + P(B) - 2P(AB)$

### 1.3.2 解

A：我们可以根据公式来

$$A\bar{B} = \bar{A} B$$
$$A - AB =  B - AB$$
于是就可以直接得到
$$A = B$$

B: A或B恰有一个发生的概率 $\rightarrow$ $P(A\bar{B} \cup \bar{A}B) = P(A) + P(B) - 2P(AB)$

# 2. 古典概型
所谓古典概型，有两个比较常见的例子，一个是彩票抽奖，或者投骰子；另一个是从有限个元素中随机抽拿几个，比如从黑球和白球的盒子里拿几个球问其中有黑球几个的这类问题，所以你不必去记背它的数学定义，记住这几个例子就可以了。

由于古典概型大体都是这两种类型，所以需要记住两个不同的概率计算方式。

## 2.1. 抽取模型
这个的概念是从一堆样本中，随机抽取一个，它归属于某种类型的概率，因此

$$P(A) = \frac{类型A总数}{总样本数}$$

## 2.2. 抽样模型
抽样，指的是从一堆样本中，随机抽取数个，问抽取方式、或排列组合方式，因此

$$C_n^m = \frac{n \times (n-1) \times \cdots \times (n-m +1)}{m!}$$
$$=\frac{n!}{m! (n-m!)}$$

例如，问从有4个黑球3个白球中，抽取3个球，正好出现2黑1白的概率:

$$ P = \frac{C_4^2C_3^1}{C_7^3} = \frac{\frac{4*3}{2*1} \frac{3}{1}}{\frac{7* 6 * 5}{3 * 2 * 1}} = \frac{18}{35}$$


需要注意的是以下情况：

* $C_m^{m-1} = C_m^1$
* $0! = 1$

## 2.3 练习题

一袋中有4白3黑球，按下列情况，求各自概率
A：从中每次取1个，有放回的取2次,  都是白球的概率。
B：从中每次取1个，无放回的取2次， 都是白球的概率。
C：有放回的取3次，恰好是1白2黑的概率


A:  
$$P(白 \cap 白) = \frac{4}{7} \times \frac{4}{7} = \frac{16}{49}$$

B:
$$P = \frac{C_4^2}{C_7^2} = \frac{\frac{4 * 3}{2}}{\frac{7 * 6}{2}} = \frac{2}{7}$$

C: 稍微有点烧脑，假设已经完成了三次取球，满足题干要求，其样本空间为 
* 【黑， 白， 黑】
* 【白， 黑， 黑】
* 【黑， 黑，白】

所以除了算独立事件的概率外，我们还要从排列组合中随机抽取一种组合，因此需要
$$P = C_3^1 P(白 \cap 黑 \cap 黑) = 3\frac{4}{7} \frac{3}{7} \frac{3}{7}= \frac{108}{343} $$

# 3. 几何概型

很简单，我直接上一道例题好了

## 3.1. 练习
如果$x \in [1, 6]$， 那么$2 < x < 5$的概率是多少

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210706001134106.png#pic_center)

答 3/5

关于几何概型的更多资料，你可以参考[百度百科](https://baike.baidu.com/item/%E5%87%A0%E4%BD%95%E6%A6%82%E5%9E%8B)的相关文献：

