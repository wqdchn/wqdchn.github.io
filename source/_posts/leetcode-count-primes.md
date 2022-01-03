---
title: LeetCode第204题Count Primes
date: 2020-02-08 09:33:25
categories: 
- 数据结构与算法
tags:
- LeetCode
- 素数
---
<!-- more -->
#### Count Primes

[Count Primes](https://leetcode.com/problems/count-primes/)给出一个数，计算小于这个数的所有的素数个数。例如，输入`10`，则`10`以内的素数有四个，分别为`2`，`3`，`5`，`7`，故返回`4`。

求素数的问题相信很多计算机专业的同学在大一刚接触编程的时候一定会碰上，求素数也不难。但是在这里是求素数的个数，一个很简单的思路是调用素数判断的函数，如果是素数则计数加一，最后返回结果。

```Java
 //Runtime: 548 ms, faster than 13.15% of Java online submissions for Count Primes.
 public static int countPrimes(int n) {
    int count = 0;
    for (int i = 1; i < n; i++) {
      if (isPrime(i)) {
        count++;
      }
    }
    return count;
  }

  public static boolean isPrime(int n) {
    if (n <= 1) {
      return false;
    }
    for (int i = 2; i * i <= n; i++) {
      if (n % i == 0) {
        return false;
      }
    }
    return true;
  }

```

但是你也看到了，这样的效率并不高，有没有办法让它更高更快更强呢，答案是有的。

首先我们知道，所有的偶数都不是素数，也就是说，我们的`n`中有一半都不是素数，即我们的`count`最多也就是`n/2`。

其次，我们只需要在剩下的一半中寻找素数，或者反向操作寻找不是素数的，然后令`count`不断地减减。

```Java
  // Runtime: 6 ms, faster than 99.32% of Java online submissions for Count Primes.
  public static int countPrimes2(int n) {
    if (n < 3) {
      return 0;
    }
    int count = n / 2;
    boolean not_prime[] = new boolean[n];

    // i从奇数开始，从奇数中取
    // i会取3，5，7，9...
    for (int i = 3; i * i < n; i += 2) {
      if (not_prime[i] == true) {
        continue;
      }
      // j从奇数开始，从奇数中取，我们要剔除的是i的奇数倍
      // j会取 3*3， 3*(3+2)，3*(3+4)，3*(3+6)...
      for (int j = i * i; j < n; j += i * 2) {
        // 找到count个奇数中的合数，count--，并标记为非素数
        if (not_prime[j] == false) {
          not_prime[j] = true;
          count--;
        }
      }
    }
    return count;
  }

```

是不是更高更快更强了呢。

#### 总结

素数真好玩。