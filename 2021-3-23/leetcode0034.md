####  [在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

***

**题目描述**

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

**进阶：**

- 你可以设计并实现时间复杂度为 `O(log n)` 的算法解决此问题吗？

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

提示：

- 0 <= `nums.length` <= $10^5$
-  -109 <= `nums[i]` <= $10^9$
-  `nums` 是一个非递减数组
-  -$10^9$ <= `target` <= $10^9$

***

**我的答案**

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        auto startIter = lower_bound(nums.begin(), nums.end(), target);
        auto endIter = upper_bound(nums.begin(), nums.end(), target);
        if(startIter==endIter) return {-1,-1};
        int left = startIter-nums.begin();
        int right = endIter-nums.begin()-1;
        return {left, right};
    }
};
```

使用`algorithm`算法库中的`lower_bound`和`upper_bound`

解法二：

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        auto range = equal_range(nums.begin(), nums.end(), target);
        if(range.first==range.second) return {-1, -1};
        int left = range.first - nums.begin();
        int right = range.second - nums.begin()-1;
        return {left, right};
    }
};
```

使用`algorithm`算法库中的`equal_range`，同时获取`lower_bound`和`upper_bound`的结果。

***

**二分解法（来自于官方题解）**

```cpp
class Solution {
public:
    int binarytSearch(vector<int>& nums, bool isLower, int target)
    {
        int left = 0, right = nums.size()-1;
        int result = nums.size();
        while(left <= right) {
            int mid =left+(right-left)/2;
            if(nums[mid]>target || (isLower&&nums[mid]==target)) {
                result =mid;
                right = mid-1;
            } else {
                left =mid+1;
            }
        }
        return result;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        int left = binarytSearch(nums, true, target);
        int right = binarytSearch(nums, false, target);
        if(left==right) return {-1, -1};
        return {left, right - 1};
    }
};
```

其实本质上是实现了`lower_bound`和`upper_bound`