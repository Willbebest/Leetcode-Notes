####  [寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

***

**题目描述**
假设按照升序排序的数组在预先未知的某个节点进行了旋转。例如，数组  `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` 。

请找出其中的最小元素

示例 1：

```
输入：nums = [3,4,5,1,2]
输出：1
```

示例 2：

```
输入：nums = [4,5,6,7,0,1,2]
输出：0
```

示例 3：

```
输入：nums = [1]
输出：1
```

提示：

- 1 <= `nums.length` <= 5000
- --5000 <= `nums[i]` <= 5000
- `nums` 中的所有整数都是 唯一 的
- `nums` 原来是一个升序排序的数组，但在预先未知的某个点上进行了旋转

***

**题意分析**

本题与[搜索旋转排序数组](https://github.com/Willbebest/Leetcode-Notes/blob/main/2021-3-15/leetcode0033.md)类似，只不过一个是要求返回目标值的的位置，此题要返回的是最小值。

`nums` 中的所有整数都是 唯一 的
可以使用遍历查找，也可以使用类似于二分查找的方式。

***

**我的解法**

解法一：

```cpp
    int findMin(vector<int>& nums) {
        auto iter = min_element(nums.begin(), nums.end());
        return *iter;
    }
```

调用库函数min_element，进行查找最小值。缺点是会遍历所有值

解法二：

对`解法一`进行优化

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int len =nums.size()-1;
        int minVal = nums[0];
        for(int i=1; i<=len; i++) {
            if(nums[i]<nums[i-1]) {
                minVal = nums[i];
                break;
            }
        }
        return minVal;
    }
};
```

通过观察可以发现，最小值有两种情况：

   - 当`nums`只有一个元素时，则这个数值就是最小值
   - 当`nums`有多个元素时，因为数组在预先未知的某个点进行了旋转，那么最小值可能会有两种情况：
        - 如果旋转后的数组与旋转前的数组一致，是递增的，那么最小值是第一个元素
        - 其他情况下，最小值一定是小于前一个元素。

***

**二分查找**

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int low =0;
        int high = nums.size() -1;
        while(low<high) {
            int mid = low +(high-low)/2;
            if(nums[mid] >nums[high]) {
                low =mid+1;
            } else {
                high = mid;
            } 
        }
        return nums[low];
    }
};
```

本来自己写了一堆代码,后来一看评论区。发现想法一致，但却可以如此精简，于是默默地搬运过来了。

当`mid`的数值大于最高位`high`的数值时，说明最小值在`mid~high`之间。
当`mid`的数值小于等于最高位`high`的数值时，说明最小值可能位于`mid`的右侧，也可能就在`mid`处。

