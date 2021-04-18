####  [合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

***

**题目描述**

给你两个有序整数数组 `nums1` 和 `nums2`，请你将 `nums2` 合并到 `nums1` 中*，*使 `nums1` 成为一个有序数组。

初始化 `nums1` 和 `nums2` 的元素数量分别为 `m` 和 `n` 。你可以假设 `nums1` 的空间大小等于 `m + n`，这样它就有足够的空间保存来自 `nums2` 的元素。

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
```

**提示：**

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`

- `1 <= m + n <= 200`
- `-109 <= nums1[i], nums2[i] <= 109`

***

**我的答案**

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int idx = m+n-1;
        int i= m-1;
        int j = n-1;
        while(i>=0 || j>=0) {
            if(i<0) {
                nums1[idx--] = nums2[j--];
            } else if(j<0) {
                nums1[idx--] = nums1[i--];
            } else {
                nums1[idx--] = nums2[j]>=nums1[i] ? nums2[j--] : nums1[i--];
            }
        }
    }
};
```

从后向前遍历，减少元素移动。

时间复杂度为O(m+n)，空间复杂度O(1)