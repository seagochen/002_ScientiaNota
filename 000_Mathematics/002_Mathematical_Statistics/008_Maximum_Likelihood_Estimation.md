最大似然估计（Maximum Likelihood Estimation, MLE）是统计学中另一种常用的参数估计方法。MLE的基本思想是选择能够最大化样本观测值联合概率密度（或概率质量）的参数值。

## 原理和解释

设 \( X_1, X_2, \ldots, X_n \) 是独立同分布的随机变量，其概率密度函数（或概率质量函数）为 \( f(x; \theta) \)，其中 \( \theta \) 是需要估计的参数。最大似然估计法的基本步骤如下：

1. **定义似然函数**：似然函数 \( L(\theta) \) 表示在参数 \( \theta \) 下观测到样本 \( X_1, X_2, \ldots, X_n \) 的联合概率。对于独立同分布的样本，似然函数定义为：

\[ L(\theta) = \prod_{i=1}^{n} f(X_i; \theta) \]

2. **取对数似然函数**：为了简化计算，通常取似然函数的对数，称为对数似然函数：

\[ \ell(\theta) = \log L(\theta) = \sum_{i=1}^{n} \log f(X_i; \theta) \]

3. **求解最大化问题**：通过求解对数似然函数的最大值，找到使对数似然函数最大化的参数 \( \theta \)。这通常需要对 \( \theta \) 求导并设导数为零，解出参数的估计值。

\[ \frac{\partial \ell(\theta)}{\partial \theta} = 0 \]

## 数学公式

假设样本 \( X_1, X_2, \ldots, X_n \) 来自分布 \( f(x; \theta) \)，则似然函数为：

\[ L(\theta) = \prod_{i=1}^{n} f(X_i; \theta) \]

对数似然函数为：

\[ \ell(\theta) = \sum_{i=1}^{n} \log f(X_i; \theta) \]

求解 \( \theta \) 的估计值：

\[ \frac{\partial \ell(\theta)}{\partial \theta} = 0 \]

## 例题

### 例题 1：正态分布的最大似然估计

假设我们有一组来自正态分布 \( N(\mu, \sigma^2) \) 的样本数据。我们想估计 \( \mu \) 和 \( \sigma^2 \)。

1. **定义似然函数**：

\[ L(\mu, \sigma^2) = \prod_{i=1}^{n} \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left( -\frac{(X_i - \mu)^2}{2 \sigma^2} \right) \]

2. **取对数似然函数**：

\[ \ell(\mu, \sigma^2) = \sum_{i=1}^{n} \left( -\frac{1}{2} \log (2 \pi \sigma^2) - \frac{(X_i - \mu)^2}{2 \sigma^2} \right) \]

3. **求解最大化问题**：
   - 对 \( \mu \) 求导并设导数为零：

   \[ \frac{\partial \ell(\mu, \sigma^2)}{\partial \mu} = \sum_{i=1}^{n} \frac{X_i - \mu}{\sigma^2} = 0 \]

   解得：

   \[ \hat{\mu} = \frac{1}{n} \sum_{i=1}^{n} X_i \]

   - 对 \( \sigma^2 \) 求导并设导数为零：

   \[ \frac{\partial \ell(\mu, \sigma^2)}{\partial \sigma^2} = -\frac{n}{2\sigma^2} + \frac{1}{2\sigma^4} \sum_{i=1}^{n} (X_i - \mu)^2 = 0 \]

   解得：

   \[ \hat{\sigma}^2 = \frac{1}{n} \sum_{i=1}^{n} (X_i - \hat{\mu})^2 \]

### 例题 2：指数分布的最大似然估计

假设我们有一组来自指数分布 \( Exp(\lambda) \) 的样本数据。我们想估计参数 \( \lambda \)。

1. **定义似然函数**：

\[ L(\lambda) = \prod_{i=1}^{n} \lambda \exp(-\lambda X_i) = \lambda^n \exp \left( -\lambda \sum_{i=1}^{n} X_i \right) \]

2. **取对数似然函数**：

\[ \ell(\lambda) = n \log \lambda - \lambda \sum_{i=1}^{n} X_i \]

3. **求解最大化问题**：
   - 对 \( \lambda \) 求导并设导数为零：

   \[ \frac{\partial \ell(\lambda)}{\partial \lambda} = \frac{n}{\lambda} - \sum_{i=1}^{n} X_i = 0 \]

   解得：

   \[ \hat{\lambda} = \frac{n}{\sum_{i=1}^{n} X_i} \]

通过上述步骤，我们可以使用最大似然估计法来估计各种分布的参数。最大似然估计法通常是参数估计的一个有效而灵活的方法，因为它在很多情况下具有良好的统计性质，比如一致性和渐近正态性。