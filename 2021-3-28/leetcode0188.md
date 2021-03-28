####  [买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

***

给定一个整数数组 `prices` ，它的第 `i` 个元素 `prices[i]` 是一支给定的股票在第 `i` 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 **k** 笔交易。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1：

```
输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```

示例 2：

```
输入：k = 2, prices = [3,2,6,5,0,3]
输出：7
解释：在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
```

**提示：**

- `0 <= k <= 100`
- `0 <= prices.length <= 1000`
- `0 <= prices[i] <= 1000`

***

**题意分析**

与121/122/123不同的是，此题中允许`k`次买卖。但解题思路与前面的没有差别。

需要注意的是此题中`0 <= k <= 100`

***

**我的答案**

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        if(k==0 || prices.size() <2) return 0; // 需要注意
        vector<int> buy(k, INT_MAX);
        vector<int> sell(k, 0);
        for(auto elem : prices) {
            buy[0] = min(buy[0], elem);
            sell[0] = max(sell[0], elem-buy[0]);
            for(int i=1; i < k; i++) {
                buy[i] = min(buy[i], elem - sell[i-1]);
                sell[i] = max(sell[i], elem - buy[i]);
            }
        }

        return sell.back();
    }
};
```

此题思路与123没有差别。唯一需要注意的是`prices`大小为空的情况。 