#### [搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

***

**题目描述**

此题与[leetcode0033搜索旋转排序数组](../2021-3-15/leetcode0033.md)题目类似，都是在旋转数组`nums`中查找特定值`target`，唯一不同的是此题中`nums`的元素可以是重复的。

***

**我的解法**

顺序遍历搜索,时间复杂度为O(n)：

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        auto iter = find(nums.begin(), nums.end(), target);
        return iter!=nums.end();
    }
};
```

二分查找查找法

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;
        while(low<=high){
            int mid =low+(high-low)/2;
            if(nums[mid]==target){
                return true;
            }
            if(nums[mid] >nums[high]) { // 在分界点的左侧
                if(target >nums[low]) {
                    low++;
                } else if(target <nums[low]){
                    low=mid+1;
                } else {
                    return true;
                }
            } else if(nums[mid]<nums[high]) { // 在分界点的右侧
                if(target>nums[high]){
                    high =mid-1;
                } else if(target < nums[high]) {
                    high--;
                } else {
                    return true;
                }
            } else { // 唯一能判断的是不在最高位
                high--;
            }
        }
        return false;
    }
};
```

二分查找法：

- 第一步：查找mid是在分界点的左侧还是右侧。分界点是指[4,5,6,0,1,2,3]中的6和0的中间分界。如何区分呢？直接通过比较mid处的值与最高位high处的值，如果大于最高位，表在分界点的左侧，如果小于与high表示在分界点的右侧。
- 第二步：当`mid`在分界点右侧，表明`mid`的右侧是连续的
  - 如果`target`小于`nums[high]`，说明`target`在`mid`的左侧或右侧，但不等于high
  - 如果`target`大于`nums[high]`，说明`target`在`mid`的左侧
  - 如果`target`等于`nums[high]`，返回`true`
- 第三步：当`mid`在分界点的左侧，表名`mid`左侧是连续的
  - 如果`target`小于`nums[low]`,说明`target`是在`mid`的右侧
  - 如果`target`大于`nums[low]`,说明`target`可能在`mid`的左侧，也可能在mid的右侧，只能确定`target`不等于`nums[low]`
  - 如果`target`等于`nums[low]`，返回true

***

**其它**

感觉没图来表述，仅凭还是有些苍白，图待补充

二分的方法感觉还能优化，留在复习时优化吧

分析时，在纸上勾勾画画，感觉思路更加清晰