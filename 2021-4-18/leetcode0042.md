####  [接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

***

给定 n 个非负整数，表示宽度为为 1 的柱子，计算按此排列的柱子，下雨之后能接多少雨水。

示例 1：

![图片1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

提示：

- `n == height.length`
- `0 <= n <= 3 * 104`
- `0 <= height[i] <= 105`

***

**题目分析**

对于下标 `i`，下雨后水能到达的最大高度等于下标 `i `两边的柱子最大高度的最小值，下标 `i`处能接的雨水量等于下标 `i`处的水能到达的最大高度减去 柱子高度。

边界条件：最左侧和最右侧的位置不会接雨水，所以当 `n<=2`时，不会有雨水。

***

**动态规划解法**

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        vector<int> left(n), right(n);
        for(int i=1; i<n; i++) {
            left[i] = max(left[i-1], height[i-1]);
        }

        for(int i=n-2; i>=0; i--) {
            right[i] = max(right[i+1], height[i+1]);
        }

        int water = 0;
        for(int i=0; i<n; i++) {
            int level = min(left[i], right[i]);
            water += max(0, level-height[i]);
        }

        return water;
    }
};
```

创建两个长度为 n 的数组 left 和 right。对于 0 ≤ i < n，left[i] 表示下标 i 左边的位置中，height 的最大高度，right[i] 表示下标 i 右边的位置中，height 的最大高度。

两个数组的其余元素的计算如下：

- 当  1 ≤ *i* ≤*n*−1 时，left[i] = max(left[i-1], height[i-1]);
- 当 0 ≤ *i* ≤*n*−2 时，right[i] = max(right[i+1], height[i+1]);

因此可以正向遍历数组 height 得到数组 left 的每个元素值，反向遍历数组 height 得到数组 right 的每个元素值。

在得到数组left 和right 的每个元素值之后，下标 i 能够接的雨水量等于 	`min(left[i], right[i]) - height[i]`

遍历每个下标位置即可得到能接的雨水总量。

时间复杂度是 O(n)，空间复杂度是O(n)