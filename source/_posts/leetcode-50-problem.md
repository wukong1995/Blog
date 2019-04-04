---
title: power(x, n)有感
date: 2019-03-31 19:40:46
tags: [算法]
categories: [算法]
description: leetcode的50题
---

这个题目是求x的n次方。

##### 错误求解
n的值可以是正值或者负值，第一想到的最简单粗暴的办法是for循环，but runtime error了。接着就放在那了，继续解决别的问题了。


##### 经过指点
经过老师的指点，思路变成了power(2, 8) = power(power(power(2, 2), 2), 2)，这样，八次就变成了三次，runtime error的问题解决了。但是让我写的，扣了好久也没扣来😅，老师给了答案
```js
var myPow = function(x, n) {
  let result = 1;
  let sum = x;

  while(n > 0) {
    if (n % 2 === 1) {
      result *= sum;
    }

    sum *= sum;
    n = parseInt(n / 2);
  }

  return result;
};

```
再接着问，加入负数的处理，扣了好久有没扣出来。

##### 优化
早上想了想如何处理负数的情况，在js语言中测试了一下`-1 / 2 = -1`， 这一看无论值为正值还是负值，最后靠近的结果都是0，于是就有了下面的代码：

```js
var myPow = function(x, n) {
  const temp = n;
  let result = 1;
  let sum = x;

  while(n !== 0) {
    if (n % 2 !== 0) {
      result *= sum;
    }

    sum *= sum;
    n = parseInt(n / 2);
  }

  if(temp < 0) return 1 / result;

  return result;
};
```
提交代码accept，一切ok。

但是shared给老师看，老师说`n % 2`那一块可能会出问题，于是查了一下“负数求模”，果然是有问题的。于是我在ruby中尝试`-1 / 2`的结果竟然等于1！！！！！！啊，真的是一个很意外的结果。

老师给了一个解答，在一段代码中加入`if(n < 0) return 1 / myPow(x, -n)`，so cool， 但是也很sad，不知道自己当时的鬼迷心窍。哎

##### 总结
负数不要进行求模运算。

将整个问题切成几块处理，在多线程的程序中，如果有3亿个数，可以使用三个线程分别去计算一亿个数的和，最后相加。

对于能合并处理的问题一定合并处理。












