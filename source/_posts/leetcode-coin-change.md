---
title: LeetCode第322题Coin Change
date: 2019-11-29 17:36:35
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 动态规划
---
<!-- more -->
#### Coin Change

[Coin Change](https://leetcode.com/problems/coin-change/)给出不同币值的硬币和一个目标金额，求组成目标金额所需最少的硬币数量，注意，这里可供取用的硬币数据量是不限制的，如果没有可用的硬币，则返回-1即可。

##### Example 1:

>Input: coins = [1, 2, 5], amount = 11
>Output: 3 
>Explanation: 11 = 5 + 5 + 1

##### Example 2:

>Input: coins = [2], amount = 3
>Output: -1

这一题使用暴力法会超时，因此需要改换策略，实际上就是要求使用动态规划来完成。动态规划最头痛的就是递推方程，要了解每一次方程的状态是如何转移的，简直让我选择放弃，好在这一题是找零钱，仿佛小学生奥数似的，稍有些趣味性，不妨挑战一下。

以示例一为例，我们现在有1，2，5三种币值的硬币，且任意取用。当我们需要11块钱的时候，我们可以选择摸11个一块钱，也可以选择摸5个两块钱和1个一块钱，当然，最少的是摸2个五块钱和1个一块钱。那么这里就存在一种规律了，比如说我们恰好摸了2个五块钱和1个一块钱，完成了任务，可是这个过程中，我们是摸了2个五块钱和1个一块钱的~~这不是废话嘛！！！~~注意了，这个2个五块钱和1个一块钱可不简单，它们分别是十块钱和一块钱~~这又是什么废话啊！！！~~
嗯，到这里暗示已经结束，该明示了。就是说，组合11块钱的任务是由组合10块钱和组合1块钱来完成的，而两个组合也恰好是最少的，不仅是组合成11块钱是最少的，他们本身组合成10块钱和组合成1块钱也是最少的。因此，我们的动态规划递推方程中，当前的组合金额`(11)`还有币值`(1,2,5)`与之前的组合金额`(10,1)`是有关联的，我们最终会推到`0`，也就是什么都不做。

简单的数学公式是这样的：
`F(11) = min(F(11-1),F(11-2),F(11-5)) + 1`
`F(11) = min(F(10),F(9),F(6)) + 1`
然后呢我们分别再求`F(10)`、`F(9)`、`F(6)`....

这个`F`函数就是我们要实现的摸硬币操作。

#### Solution

下面是`Java`的代码：

注意：外层循环表示可用的硬币组合，内层循环则表示在当前可用的硬币组合下，完成目标金额数所需要的硬币数量。

```Java
class Solution {
  public static int coinChange(int[] coins, int amount) {
    if (amount == 0) {
      return 0;
    }
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);
    dp[0] = 0;
    for (int coin : coins) {
      for (int i = coin; i < amount + 1; i++) {
        dp[i] = Math.min(dp[i], dp[i - coin] + 1);
      }
    }
    return dp[amount] != amount + 1 ? dp[amount] : -1;
  }
}
```

下面是`Python`的代码：

```Python
class Solution:
    def coinChange(self, coins, amount):
        dp = [0] + [float('inf')] * amount

        for coin in coins:
            for i in range(coin, amount + 1):
                dp[i] = min(dp[i], dp[i - coin] + 1)

        return dp[-1] if dp[-1] != float('inf') else -1 
```

#### 总结

第322题是中等题，除了找零钱有些趣味，动态规划实在打扰了。