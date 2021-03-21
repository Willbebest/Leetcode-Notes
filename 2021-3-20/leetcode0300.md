####  [最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

**提示：**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

**进阶：**

- 你可以设计时间复杂度为 `O(n2)` 的解决方案吗？
- 你能将算法的时间复杂度降低到 `O(n log(n))` 吗?

***

**题意分析**

本题的子序列是指删除部分元素而不改变其余元素在数组中的顺序。

要求查找的是最长递增子序列

数组的长度大于 1

***

**我的解法**

没有，写了一个小数，发现方向错了，最后还是理解网上的 Most Votes答案。不过理解答案也理解了很久，用一种方法也看了多篇。

***

**网上优秀解法**

动态规划解法

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int len = nums.size();
        vector<int> dp(len, 1);
        for(int i=0; i<len; i++) {
            for(int j=0; j<i; j++) {
                if(nums[j]<nums[i]) {
                    dp[i] = max(dp[i], dp[j]+1);
                }
            }
        }
        return *max_element(dp.begin(), dp.end());
    }
};
```

> 使用 `dp` 数组记录已经遍历过的位置的最大子序列长度, 而`dp[k]`长度的子序列的末位元素是`nums[k]`。  
> 当 `idx`大于 k 时，通过比较`nums[k]`的值与`nums[idx]`的大小，就知道`dp[k]`所代表的子序列是否能和`nums[idx]`组成子序列。而动态规划就是在`0~idx-1`的所有子序列中选出最长的子序列长度进行记录在`dp[idx]`中。  
> 遍历计算 `dp[p]`列表需 O(n)，计算每个 `dp[k]`需要 O(n)。时间复杂度为O($n^2$)。空间复杂度为 O(n)

动态规划 + 二分查找的解法

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> res;
        for(int i=0; i<nums.size(); i++) {
            auto it = std::lower_bound(res.begin(), res.end(), nums[i]);
            if(it==res.end()) res.push_back(nums[i]);
            else *it = nums[i];
        }
        return res.size();
    }
};
```

> 看了好几篇，网上对于这种方法为什么可行的解读大多类似，感觉不是很好理解，说说我自己的理解。
>
> 先说一下代码实现，然后再说这种方法为什么可行
>
> 代码实现：
> - 使用一个数组 `res[k]` 记录 `nums` 中遍历过的元素中，组成长度为`k+1`的的所有子序列中末尾元素最小的元素值。res是一个递增序列
> - 在遍历`nums`的过程中, 当遍历到索引为 i 的 元素时，判断 res 中是否存在元素val 大于等于`nums[i]`： 
>   - 如果存在直接使用`nums[i]`替换 val 的位置；
>   - 如果不存在，即证明res中所有的元素都小于`nums[i]`，添加`nums[i]`的值到res的尾部
>
> - 最后返回 res 的长度
> - lower_bound的底层实现是二分查找，时间复杂度为O(n)
> - res的元素并不代表当前的最长子序列本身，但是长度代表最长子序列长度
>
> 为什么可行：
>
> - 上面已经说了`res[k]` 记录 `nums` 中遍历过的元素中，组成长度为`k+1`的的所有子序列中末尾元素最小的元素值。res的长度代表最长子序列长度
>
> - 当我们遍历 `nums` 到为索引`i` 的元素时，对res 中是否存在元素val 大于等于`nums[i]`，由上面的代码，可以知道 res 的长度代表最长子序列的长度。只有当`nums[i]`大于res的末尾元素时，向res中添加`nums[i]`， res的长度加1。
>
>   当`nums[i]`小于等于res的末尾元素时，只会进行替换，不会改变长度。那既然这样为啥还要替换？原因是因为当每个子序列的尾部元素最小时，最终的子序列的长度才会最大。
>
>  - 那我们替换res最后一个元素时，会不会影响res长度的递增（添加元素到res）？
>
>     - 我们已经知道只有当`nums[i]`大于res的末尾元素时，才会向res中添加`nums[i]`， res的长度加1。
>     - 当替换res的最后一个元素时，并不影响下一个元素`nums[i]`进入res所形成的子序列。假设因为当`nums`中出现大于`res末尾元素`的元素时，`nums`中索引为`0~i`范围内一定会有一个以当前res的末尾元素结尾的子序列，且这个子序列的长度为res的长度。所以不会影响最终的长度

对于`动态规划 + 二分查找的解法`的代码，我自己优化了一下

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp{nums[0]}; // 记录的是序列，不是子序列长度
        int len = nums.size();
        for(int i = 1; i< len; i++) {
            if(nums[i] > dp.back()) {
                dp.push_back(nums[i]);
            } else if(nums[i] < dp.back()){
                auto iter = lower_bound(dp.begin(), dp.end(), nums[i]); // 函数的时间复杂度为O(lgN)
                *iter = nums[i];
            }   
        }
        return dp.size();
    }
};
```

当`nums[i] > dp.back()`直接添加`nums[i]`到`dp`中，减少一次lower_bound。lower_bound作用于有序的容器，内部实现是二分查找，时间复杂度为`O(logn)`。

所以最终的时间复杂度为`O(nlogn)`