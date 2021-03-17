####  [寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

***

本题与[leetode153](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)的唯一区别是`nums`的数值是可以重复的。

***

**我的解法**

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        auto iter = min_element(nums.begin(), nums.end());
        return *iter;
    }
};
```
本题使用这种方法，时间复杂度和空间复杂度都小很多。而在`leetcode153`使用本方法，时间复杂度是本题的 3 倍为 `12ms`，本体是 `4ms`。难道是由于元素是否唯一的因素影响的？我查了一下`min_element`就是朴实的遍历实现。当然也可能是测试用例长短的原因。
```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int low =0;
        int high = nums.size() -1;
        while(low < high) {
            int mid = low +(high-low)/2;
            if(nums[mid] > nums[high]) {
                low =mid+1;
            } else {
                high = mid;
            } 
        }
        return nums[low];
    }
};
```

通过观察可以发现，最小值有两种情况：

   - 当`nums`只有一个元素时，则这个数值就是最小值
   - 当`nums`有多个元素时，因为数组在预先未知的某个点进行了旋转，那么最小值可能会有两种情况：
     - 如果旋转后的数组与旋转前的数组一致，是递增的，那么最小值是第一个元素
     - 其他情况下，最小值一定是小于前一个元素。

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
            } else if(nums[mid] < nums[high]){
                high = mid;
            } else {
                high = high -1;
            }
        }
        return nums[low];
    }
};
```

在使前`leetcode153`前两种解法，`pass`了本题之后，我把`leetcode153`的第三种解法写在了答题框内并提交，发现部分用力不能通过。于是根据用例的提示修改，得出上述解法。与`leetcode153`解法的不同在于`nums[mid]==nums[high]`的部分。当`nums[mid]=nums[high]`时，说明low~mid以及high的值相同或mid~high的值相同。使用`low=mid+1`和`high=high-1`以及`low=mid`都能很好的解决`[3,3,1,3]`的情况，但是`[1,3,3]`的情况下，`low=mid+1`不能通过，`low=mid`严重超时。只有`high=high`符合情况。可能还有一些情况没有考虑到，之后复习的时候再想想。