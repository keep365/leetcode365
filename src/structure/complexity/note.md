# 算法复杂度

数据结构和算法解决的是 `快` 和 `省` 的问题，执行效率是算法的一个重要指标，衡量执行效率通过 `时间复杂度` 和 `空间复杂度`。

[下篇，最好、最坏、平均、均摊分析](./compare.md)

## 事后统计法

把代码跑一遍，通过统计，监控，得出算法的速度和内存占用。

- 依赖测试环境，同一套代码不同的机器结果不一样
- 受数据结构和规模影响，不准确

## 大 O 复杂度表示法

不依赖环境和数据，肉眼可得的数学统计法。

从 CPU 角度看，每行代码执行着类似的操作：读数据-运算-写数据。假设每行代码的执行时间一致，都为 t，那么以下代码的总执行时间 = (2n+2)\*t。总执行时间 T(n) 与 每行代码执行次数 成正比。

```cpp
 int cal(int n) {
   int sum = 0; // t
   int i = 1; // t
   for (; i <= n; ++i) { // n*t
     sum = sum + i; // n*t
   }
   return sum;
 }
```

```cpp
// (2n^2+2n+3)*t
 int cal(int n) {
   int sum = 0; // t
   int i = 1; // t
   int j = 1; // t
   for (; i <= n; ++i) { // n*t
     j = 1; // n*t
     for (; j <= n; ++j) { // n*n*t
       sum = sum +  i * j; // n*n*t
     }
   }
 }
```

通过以上分析得出，所有代码的 `执行时间` 和每行代码的 `执行次数n` 成正比

```
T(n)=O(f(n))

T(n) 代码执行时间
f(n) 每行代码执行次数总和
O T(n) 和 f(n) 成正比
```

第一个例子，`T(n)=O(2n+2)`，第二个例子，`T(n)=O(2n^2+2n+3)`。

这个就是 `大 O 时间复杂度表示法`，并不表示执行的具体时间，而是 `执行时间随数据规模增长的变化趋势`，也叫 `渐进时间复杂度`，简称 `时间复杂度`。

当 n 很大时，公式中的低阶、常量、系数并不影响增长趋势，可以忽略，只需要记录最大的一个量级。

第一个例子，`T(n)=O(n)`，第二个例子，`T(n)=O(n^2)`。

## 时间复杂度分析

- 只关注循环执行次数最多的代码
- 加法法则：总复杂度=量级最大的代码复杂度
  - 只要是已知的数，和 n 无关，都是常量级复杂度，n 无限大时，可以忽略，对增长趋势没有影响。
- 乘法法则：嵌套代码复杂度=嵌套内外的代码复杂度的乘积。

## 常见的复杂度

![complexity](img/3723793cc5c810e9d5b06bc95325bf0a.jpg)

![graph](img/497a3f120b7debee07dc0d03984faf04.jpg)

- 非多项式量级
  - 非多项式量级的算法问题叫作 NP（Non-Deterministic Polynomial，非确定多项式）问题。
  - n 的规模变大，执行时间无限增长，低效算法。
  - O(2^n) O(n!)
- 多项式量级

### O(1)

常量级，执行时间不随 n 的增长而增长。一般来说，只要不存在递归，循环，成千上万行代码也是 `O(1)`，与 n 无关。

### O(n?logn)

对数阶

```cpp
i=1;
while (i <= n) {
 i = i * 2;
}
```

```
变量 i 的取值是个等比数列

T(n)= i=i*2 执行次数，假设 n=100
i=1,2,4,8,16,32,64,128中断
一共执行了 7 次(2^0 - 2^6)

i=1 2^0=n
i=2 2^2=n
i=4 2^4=n
...
i=64 2^6=n
i=128 2^7=n 中断
...
i=x 2^x=n

由上面的推断，代码执行次数和 x 成正比
x=log₂n
O(log₂n)
```

```cpp
i=1;
while (i <= n) {
 i = i * 3; // O(log₃n)
}
```

`O(log₃n)=log₃2 * log₂n` 去掉常量 `log₃2` 等于 `O(log₂n)`，所以可以忽略 `log 的 底`，都以 `O(logn)` 表示。

### O(m+n) O(m\*n)

复杂度由 `两个数据的规模` 决定

```cpp
int cal(int m, int n) {
  int sum_1 = 0;
  int i = 1;
  for (; i < m; ++i) {
    sum_1 = sum_1 + i;
  }

  int sum_2 = 0;
  int j = 1;
  for (; j < n; ++j) {
    sum_2 = sum_2 + j;
  }

  return sum_1 + sum_2;
}
```

m 和 n 的规模都不确定，都有影响，`O(m+n)`。相应的乘法为 `O(m*n)`。

## 空间复杂度分析

渐进空间复杂度，表示算法的 `存储空间` 和 `数据规模` 之间的 `增长关系`。常用的为 `O(1)` `O(n)` `O(n^2)`。

```cpp
void print(int n) {
  int i = 0;
  int[] a = new int[n]; // 申请了大小为 n 的数组
  for (i; i <n; ++i) {
    a[i] = i * i;
  }

  for (i = n-1; i >= 0; --i) {
    print out a[i]
  }
}
```