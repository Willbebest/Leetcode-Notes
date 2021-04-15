####  [买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

***

**题目描述**

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

示例 1
```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
   注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

示例 2
```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

提示：

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

***

**题目分析**

很明显这是一道动态规划。需要注意的地方：`只能买卖一次`。求的是最大利润。

***

**我的答案**

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice = INT_MAX;
        int maxPro = 0;
        size_t len = prices.size();
        for (size_t i = 0; i < len; i++) {
            minPrice = min(minPrice, prices[i]);
            maxPro = max(maxPro, prices[i] - minPrice);
        }
        return maxPro;
    }
};
```

时间复杂度O(n)，空间复杂度O(1)

获得利润的意思是在买入之后再卖出，用卖出的价格减去买入的价格，就是利润。所以利润最大的情况是买入价格与卖出价格相差最大的结果。

在`prices[i]`时卖出获得最大利润的情况，一定是在`prices[i]`之前以最小价格买入。依次遍历`prices`。在遍历的同时记录遍历过的元素中最小值 `minPrice`。每遍历一个元素，使用`prices[i] - minPrice`可以获得以`minPrice`买入，`prices[i]`卖出获得最大利润。使用`maxPro`记录不同元素卖出时所获利润中的最大利润。

***

动态规划解法

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        int dp[len][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for(int i=1; i<len; i++) {
            dp[i][0] = max(dp[i-1][0], prices[i]+dp[i-1][1]);
            dp[i][1] = max(dp[i-1][1], -prices[i]);
        }

        return dp[len-1][0];
    }
};
```

时间复杂度O(n)，空间复杂度O(n)