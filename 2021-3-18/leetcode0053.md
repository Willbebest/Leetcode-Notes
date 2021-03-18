#### [最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

***

**题目描述**

给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例 1：

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

示例 2：

```
输入：nums = [1]
输出：1
```

示例 3：

```
输入：nums = [-1]
输出：-1
```

进阶：如果你已经实现复杂度为 `O(n)` 的解法，尝试使用更为精妙的 **分治法** 求解。

***

**我的解法**

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxVal =  nums[0];
        int curSum =0;
        for(auto& elem : nums) {
            curSum += elem;
            maxVal =max(maxVal, curSum);
            if(curSum < 0) {
                curSum=0;
            } 
        }
        return maxVal;
    }
};
```

我在纸上列出了几种情况，发现：负数越加越小，正数越加越大。所以当前连续数组和为负数时，需要清零，重新计算连续数组和。同时也需要一个变量记录最大的连续数组和。

***

**其它**

进阶的分治法，我还没弄懂分治法是啥，等我弄懂再来搞。同时也被评论区的动态规划概念搞蒙圈了。打算找个书来瞅瞅，把这两个概念理理。



