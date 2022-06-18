---
title: 斐波那契数列
date: 2021-03-13 17:23:00
math: true
---

# 斐波那契数列

斐波那契数列（Fibonacci sequence），又称黄金分割数列，因数学家莱昂纳多·斐波那契（Leonardo Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”。

我们都知道兔子的繁殖能力是惊人的，如下表：

| 所经过的月数 |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |  10  |  11  |  12  |
| :----------: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| 兔子的总对数 |  1   |  1   |  2   |  3   |  5   |  8   |  13  |  21  |  34  |  55  |  89  | 144  |

用数学函数来定义

$$\begin {array}{c}

F(n)=\begin{cases} 1， 当n=1\\

1， 当n=2\\

F(n-1)+F(n-2), 当n>2\end{cases}

\end {array}$$

## 通过递归实现

### python函数示例

```python
def fibonacci(n):
    if n < 1:
        print("输入错误！请输入大于1的整数")
        return -1
    elif n == 1 or n == 2:
        return 1
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)

result = fibonacci(20)
if result != -1:
    print('总共有%d对小兔崽子！' % result)

总共有6765对小兔崽子！
```

### Java函数示例

```java
public static int fibonacci1(int n) {
	if (n < 1) {
		return -1;
	}else if (n ==1 || n == 2) {
		return 1;
	}

	if(n > 2){
        return fibonacci1(n - 1) + fibonacci1(n - 2);
    }
    return -1;
}
```

:::info

在Java中，由于int类型的取值范围有限，最大值/最小值为2^31^，即范围为`-2147483648 ~ 2147483648`，当n>46的时候，会发生取值范围溢出的情况，所以这里如果想要验证n>46时的计算耗时情况，需将返回值类型`int`改为`long`。

:::

## 通过迭代实现

### python函数示例

```python
def fibonacci(n):
    n1 = 1
    n2 = 1
    n3 = 1

    if n < 1:
        print('输入有误！')
        return -1

    while (n - 2) > 0:
        n3 = n2 + n1
        n1 = n2
        n2 = n3
        n -= 1

    return n3

result = fibonacci(20)

if result != -1:
    print('总共有%d对小兔崽子！' % result)

总共有6765对小兔崽子！
```

### Java函数示例

```java
public static long fibonacci2(int n) {
    // 最高支持 n = 92 ，否则超出 Long.MAX_VALUE
    if (n < 1 || n > 92) {
		return -1;
	}else if (n ==1 || n == 2) {
		return 1;
	}

	long a =1l, b= 1l, c =0l;
	for (int i = 0; i < n - 2; i++) {
		c = a + b;
		a = b;
		b = c;
	}
	return c;
}
```

:::warning

假如 n 的数值较大，使用递归的方式效率很低，速度较慢，例如当`n>=40`时，会发现计算时间明显变长，而使用迭代的方式速度比较快，所以++不是什么都用递归都是好的++{.wavy}。

:::
