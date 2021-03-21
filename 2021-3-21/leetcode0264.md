#### [ 丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/)

***

**题目描述**

编写一个程序，找出第 `n` 个丑数。

丑数就是质因数只包含 `2, 3, 5` 的**正整数**。

**示例:**

```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```

**说明:** 

1. `1` 是丑数。
2. `n` **不超过**1690。

***

**题意分析**

丑数只能是1，2，3，5相乘的正整数。

这列的求第 n 个丑数，表示将所有丑数排序的第 n 个丑数。

***

**动态规划**

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        int idx2=0, idx3=0, idx5=0;
        vector<int> dp(n, 0);
        dp[0]=1;
        int t2 , t3, t5;
        for(int i=1; i<n; i++) {
            t2 = dp[idx2]*2;
            t3 = dp[idx3]*3;
            t5 = dp[idx5]*5;
            dp[i] = min(min(t2, t3), t5);
            if(dp[i]==t2) {
                idx2++;
            } 
            if(dp[i]==t3) {
                idx3++;
            } 
            if(dp[i] == t5) {
                idx5++;
            }
        }
        return dp[n-1];
    }
};
```

使用 `dp` 记录所有丑数。因为丑数是1，2，3，5的乘积，所以可以看出丑数是丑数的乘积。使用三个指针分别接下来记录从小到大的丑数与2，3，5相乘的位置。

每次向 `dp` 中添加最小的乘积。

代码中使用`if`，而未使用`else if`的原因是排除类似于2\*3和3\*2所得抽数相同的情况

**优先队列（来自网上）**

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        priority_queue <int ,vector<int>,greater<int>> q;
        int answer=1;
        for (int i=1;i<n;++i)
        {
            q.push(answer*2);
            q.push(answer*3);
            q.push(answer*5);
            answer=q.top();
            q.pop();
            while (!q.empty() && answer==q.top())
                q.pop();
        }
        return answer;
    }
};
```

利用了priority_queue的自动排序功能，同时使用while循环去重。原理还是：丑数是1，2，3，5的乘积，丑数是丑数的乘积。