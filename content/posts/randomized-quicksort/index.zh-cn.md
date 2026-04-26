---
title: "快速排序中的随机性"
date: 2025-12-31T00:50:21-0500
categories: ["notes"]
tags:
  - "algorithms"
  - "quicksort"
  - "randomized algorithms"
series:
  - "Randomized Algorithms"
series_order: 1

heroStyle: "thumbAndBackground"
---

<style>
.custom-highlight {
  background: #000000;
  color: #ffffff;
  padding: 0.2em;
  border-radius: 0.2em;
}

.dark .custom-highlight {
  background: #ffffff;
  color: #000000;
  padding: 0.2em;
  border-radius: 0.2em;
}
</style>

{{< katex >}}

> 本文默认你已经了解基础快速排序算法、BigO记号，以及如何读懂概率与期望推导。如果这些概念不熟，先复习一下确定性快速排序和期望值的基础，会更容易理解下面的证明。

## 算法

随机化快速排序就是在每次子数组中**均匀随机**选择pivot的快速排序。随机性可以显式实现（随机选一个索引），也可以隐式实现（先打乱数组，再用首元素做pivot）。两种方式的算法结构一致。

```text
R-QUICKSORT(A):
  if |A| <= 1: return A
  choose pivot p uniformly at random from A
  partition A into L = {x < p}, E = {x = p}, G = {x > p}
  return R-QUICKSORT(L) + E + R-QUICKSORT(G)
```

我们只需分析比较次数，排序算法的比较次数决定了时间复杂度。最坏情况仍然是 \\( O(n^2) \\)，但随机化使**期望**为 \\( O(n \log n) \\)，而精确最大比较次数路径的概率会阶乘级降低。

算法里的相等分区可以处理重复键。下面的比较次数证明采用的是标准的互异键模型。

## 复杂度分析

假设所有键互不相同，并按排序顺序记为 \\( z_1 < z_2 < \dots < z_n \\)。令 \\( X_{ij} \\) 为指示变量，表示 \\( z_i \\) 与 \\( z_j \\) 是否被比较过（\\( i < j \\)）。

比较次数总和为

$$
C = \sum_{1 \le i < j \le n} X_{ij}.
$$

由期望的线性性，

$$
\mathbb{E}[C] = \sum_{1 \le i < j \le n} \mathbb{E}[X_{ij}] = \sum_{1 \le i < j \le n} \Pr[X_{ij} = 1].
$$

**声明：** \\( z_i \\) 与 \\( z_j \\) 被比较，**当且仅当**集合 \\( \{z_i, z_{i+1}, \dots, z_j\} \\) 中第一次被选为pivot的是 \\( z_i \\) 或 \\( z_j \\)。

**证明：** 考察该连续区间内第一个被选中的pivot。如果pivot是 \\( z_k \\)， \\( i < k < j \\)，那么 \\( z_i \\) 与 \\( z_j \\) 会被分到不同分区，之后不再相见。若第一个pivot是 \\( z_i \\)（或 \\( z_j \\)），在那次划分中 \\( z_i \\)（或 \\( z_j \\)）会与区间内所有元素比较。证毕。

pivot在区间中均匀随机选取，因此

$$
\Pr[X_{ij} = 1] = \frac{2}{j - i + 1}.
$$

于是

$$
\mathbb{E}[C] = \sum_{1 \le i < j \le n} \frac{2}{j - i + 1}.
$$

令 \\( k = j - i + 1 \\)，带入化简，

$$
\mathbb{E}[C] = \sum_{k=2}^{n} (n - k + 1) \cdot \frac{2}{k}
= 2 \sum_{k=2}^{n} \frac{n - k + 1}{k}.
$$

利用调和数 \\( \sum_{k=1}^{n} \frac{1}{k} = H_n \\)，得到期望时间复杂度

$$
\mathbb{E}[C] = 2(n+1)H_n - 4n = O(n \log n).
$$

## 精确最坏路径的概率？

精确的最大比较次数路径发生在每次pivot都是当前子数组中的极值时，也就是最小值或最大值。对大小为 \\( n \\) 的输入，这条路径的概率为

$$
\Pr[\text{最坏情况}] = \prod_{k=2}^{n} \frac{2}{k} = \frac{2^{n-1}}{n!},
$$

这是阶乘级别的小概率。<span class="custom-highlight">当 \\( n = 10 \\) 时约为 \\( 2^9/10! \approx 0.0141\\% \\)，当 \\( n = 20 \\) 时约为 \\( 2^{19}/20! \approx 0.0000000000216\\% \\)。</span> 虽然算法仍保留最坏情况上界，但随机选pivot把这条最大比较次数路径推到了概率分布的极远尾部。

## 绝大部分时候都是 \\(O(n \log n)\\)？

如果一个pivot让左右两个递归子问题的规模都不超过当前子问题的 \\( 3/4 \\)，我们称它为**好**pivot。沿着任意一个固定元素的递归路径，经过 \\( t \\) 次好pivot后，包含该元素的子问题规模至多为 \\( (3/4)^t n \\)。要使其 \\( \le 1 \\)，即递归结束，需要 \\( t \ge c_0 \log n \\)（\\( c_0 \\) 为常数）。

每次pivot为好的概率至少是 \\( 1/2 \\)（只要落在中间1/4至3/4这一半即可），而且无论之前发生了什么，这个下界都成立。考虑一个固定元素的递归路径，令 \\( G \\) 为这条路径前 \\( m \\) 次pivot选择中的好pivot个数。虽然这些事件不独立，但每次pivot在条件概率下至少有 \\( 1/2 \\) 的概率是好pivot，因此 \\( G \\) 随机支配一个 \\( \text{Binomial}(m, 1/2) \\) 随机变量。

取 \\( m = \frac{2c_0}{1 - \delta} \log n \\)，其中 \\( \delta \in (0,1) \\)。则均值为 \\( \mu = m/2 = \frac{c_0}{1 - \delta} \log n \\)，坏事件（进展不足）为 \\( G < c_0 \log n = (1 - \delta)\mu \\)。对二项分布应用 Chernoff bound，

$$
\Pr[G < (1 - \delta)\mu] \le \exp\left(-\frac{\delta^2 \mu}{2}\right).
$$

因此

$$
\Pr[G < c_0 \log n] \le \exp\left(-\frac{\delta^2 \mu}{2}\right) = n^{-\frac{\delta^2}{2(1 - \delta)}c_0}.
$$

对单个元素而言，在 \\( m = \Theta(\log n) \\) 次递归调用后仍处于非平凡子问题中的概率是多项式级别的小。选择合适的常数 \\( \delta \\) 和 \\( c_0 \\)，让指数至少为 2，就可以把这个概率压到至多 \\( n^{-2} \\)。再对所有 \\( n \\) 个元素做并集界，有

$$
\Pr[\text{任意元素所在路径耗时过长}] \le O(n) \cdot n^{-2} = O(n^{-1}),
$$

当 \\( n \to \infty \\) 时趋于 0。由于每一层递归划分的都是互不重叠的子数组，总规模至多为 \\( n \\)，所以 \\( O(\log n) \\) 的递归深度会推出高概率下总划分工作量为 \\( O(n \log n) \\)。

<span class="custom-highlight">直观上讲，这和连续抛硬币类似。例如 40 次抛掷的期望是 20 次正面，而出现少于 10 次正面的概率只有约 0.03\%。随着 \\( n \\) 的增大，偏离期望稍微大一点的事件，发生的可能性都极小。</span> 同理，随机化快速排序在 \\( \Theta(\log n) \\) 步之后仍未取得足够进展的概率**以多项式速度衰减**，且指数随 \\( c_0 \\) 线性增长。

## 总结

随机化在不改变算法结构的情况下，让快速排序在**期望**下是永远高效的。时间复杂度为 \\( O(n \log n) \\) ，同时递归深度显著超出 \\( \Theta(\log n) \\) 的概率极小。

## 参考文献

- Rajeev Motwani and Prabhakar Raghavan, *Randomized Algorithms*, Cambridge University Press, 1995, Chapter 1.
