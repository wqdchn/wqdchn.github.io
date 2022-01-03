---
title: LeetCode股票买卖系列
date: 2020-03-06 09:01:48
categories: 
- 数据结构与算法
tags:
- LeetCode
- 动态规划
---

#### Best Time to Buy and Sell Stock

来看`LeetCode`的股票买卖系列问题的第一题[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)。这一题要求在一系列股票价格中，通过一次买卖交易获得最大的收益。假设有这么一组股票价格`[7,1,5,3,6,4]`，我们只能在买进之后再卖出来赚取收益。很显然，我们需要在第二天的时候以`1`元的价格买进，然后在第五天的时候以`6`元的价格卖出，这样得到最大的收益`5`元。这可以用暴力法通过两层`for`循环遍历实现，但是还有一个比较好的办法，更加贴近人的思维。

首先，我们知道，买进时的股价是代价，卖出时的股价和代价之间的差价是收益。我们的目的是希望代价最低，收益最高。以上面的股价序列为例，如果我们在第一天以`7`元的价格买入股票，这时代价是`7`，收益是`0`。到了第二天，现在的股价是`1`，意味着此时代价可以是`1`，而收益是`1-7`等于`-6`。所以我们更新一下代价，但是不更新收益，此时代价是`1`，收益是`0`。从人类的思维来说，这是选择在第二天买进，然后等等看。到了第三天，现在的股价是`5`，此时的代价可以是`5`，而收益是此时的股价减去之前的代价`5-1`等于`4`。所以我们选择更新收益，但是不更新代价，此时代价是`1`，收益是`4`。以此类推，有更小的代价就更新代价，有更高的收益就更新收益。

```Java
class Solution {

  public int maxProfit(int[] prices) {
    if (prices.length < 2) {
      return 0;
    }

    int cost = Integer.MAX_VALUE;
    int profit = 0;

    for (int price : prices) {
      cost = Math.min(cost, price);
      profit = Math.max(profit, price - cost);
    }

    return profit;
  }
}
```

#### Best Time to Buy and Sell Stock III

来看`LeetCode`的股票买卖系列问题的第三题[Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)。这一题要求在一系列股票价格中获得最大的收益，最多可以通过两次买卖交易，但是需要第一次买卖交易完成之后才能进行第二次买卖交易。

与第一题类似，只是可以多一次交易。进行第二次交易时，我们可以把第一次交易的收益当做代价。

```Java
class Solution {

  public int maxProfit(int[] prices) {
    if (prices.length < 2) {
      return 0;
    }

    int cost1 = Integer.MAX_VALUE;
    int profit1 = 0;

    int cost2 = Integer.MAX_VALUE;
    int profit2 = 0;

    for (int price : prices) {

      cost1 = Math.min(cost1, price);
      profit1 = Math.max(profit1, price - cost1);

      cost2 = Math.min(cost2, price - profit1);
      profit2 = Math.max(profit2, price - cost2);

    }

    return profit2;
  }
}
```

#### 总结

股市有风险，入行需谨慎。股民如何在最好的时机买进股票，并在最好的时机卖出股票，这是一个玄学问题。