####  [寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

***

**题目描述**

给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

示例 1：

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

示例 2：

```
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

示例 3：

```
输入：nums1 = [0,0], nums2 = [0,0]
输出：0.00000
```

示例 4：

```
输入：nums1 = [], nums2 = [1]
输出：1.00000
```

提示：

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`

**进阶：**你能设计一个时间复杂度为 `O(log (m+n))` 的算法解决此问题吗？

***

**题意分析**

1、`-106 <= nums1[i], nums2[i] <= 106`， 元素相加不会溢出

2、返回值为：

- 如果 m+n 的结果是偶数，返回两个数组合并排序后中间位置两个元素的均值
- 如果 m+n 的结果是奇数，返回两个数组合并排序后中间位置元素
- 返回值为 double

**我的答案**

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        nums1.insert(nums1.end(), nums2.begin(), nums2.end());
        if(nums1.empty()) {
            return 0.0;
        }
        sort(nums1.begin(), nums1.end());
        int mid =nums1.size() / 2;
        return nums1.size() % 2 ? nums1[mid] : (nums1[mid]+nums1[mid-1])/2.0;
    }
};
```

暴力法：时间复杂度为O((m+n)lg(m+n))， 空间复杂度为(m+n)

使用一个数组将所有的元素存在一起组成一个新数组，然后排序，根据新数组的长度，判断返回结果。

进一步优化

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector<int> numsAll;
        int i=0, j=0;
        while(i<nums1.size()||j<nums2.size()) {
            if(i>=nums1.size()) {
                numsAll.push_back(nums2[j++]);
            } else if(j >= nums2.size()) {
                numsAll.push_back(nums1[i++]);
            } else {
                int temp = nums1[i] <= nums2[j] ? nums1[i++]:nums2[j++];
                numsAll.push_back(temp);
            }
        }

        if(numsAll.empty()) return 0.0;
        int mid = numsAll.size()/2;
        return numsAll.size()%2==0 ? (numsAll[mid]+numsAll[mid-1])/2.0 : numsAll[mid];
    }
};
```

在排序与构建数组的地方进行优化。通过依次遍历原有有序的两个数组，然后进行数组构建。

时间复杂度为 O(m+n)， 空间复杂度为O(m+n)

**log(m+n)解法**

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();
        int k = (m + n + 1) / 2;
        if(m>n) return findMedianSortedArrays(nums2, nums1);
        int left = 0;
        int right = m;
        while(left < right) {
            const int count1 = left+(right-left)/2;
            const int count2 = k - count1;
            if(nums1[count1] < nums2[count2 - 1]) {
                left = count1 + 1;
            } else {
                right = count1;
            }
        }

        const int count1 = left; // nums1 取元素个数
        const int count2 = k - left; //nums取元素个数

        int result1 = max(count1<=0?INT_MIN : nums1[count1-1],
                          count2<=0?INT_MIN : nums2[count2-1]);
        if((m+n)%2) return result1;
        int result2 = min(count1>=m?INT_MAX : nums1[count1],
                          count2>=n?INT_MAX : nums2[count2]);

        return (result1+result2)*0.5; 
    }
};
```

