#### [搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

***

**题目描述**
整数数组 `nums` 按升序排列，数组中的值 互不相同 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k（0 <= k < nums.length）`上进行了 旋转，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 从 0 开始 计数）。例如，` [0,1,2,4,5,6,7] `在下标 3 处经旋转后可能变为` [4,5,6,7,0,1,2] `。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的索引，否则返回 `-1` 。

示例 1：

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

示例 2：

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

提示：
  - 1 <= nums.length <= 5000
  - -$10^4$ <= nums[i] <= $10^4$
  - nums 中的每个值都 独一无二
  - nums 肯定会在某个点上旋转
  - -$10^4$ <= target <= $10^4$

进阶：你可以设计一个时间复杂度为 `O(log n)` 的解决方案吗？

***
**题意分析**
可以直接正常遍历数组进行遍历查找。
进阶的解法可以利用二分查找的原理进行查找。

***

**我的解法**
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        auto iter = find(nums.begin(), nums.end(), target);
        return iter ==nums.end()? -1:iter-nums.begin();
    }
};
```
利用 find 函数进行遍历查找。

***

**进阶解法**
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size()-1;
        while(low<=high){
            int mid = low + (high-low) / 2;
            if(nums[mid]>=nums[low]) {
                if(nums[mid]>target) {
                    if(target >= nums[low]) {
                        high = mid-1;
                    } else {
                        low = mid + 1;
                    }
                } else if(nums[mid]<target) {
                    low = mid+1;
                } else {
                    return mid;
                }
            } else if(nums[mid]<nums[low]) {
                if(target == nums[mid]) {
                    return mid;
                }  else if(target <= nums[high]) {
                    if(target > nums[mid]) {
                        low = mid+1;
                    } else {
                        high = mid-1;
                    }
                } else {
                    high= mid-1;
                }
            }
        }
        return -1;
    }
};
```
利用二分查找的思想，需要判断多种情况。时间复杂度`O(log n)`

建议写之前，在纸上将多种情况列出来。