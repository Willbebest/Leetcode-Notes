####  [螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

***

**题目描述**

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

**示例 1：**

![示例1](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

提示：

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

***

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        int up = 0, down = matrix.size() - 1;
        int left = 0, right = matrix[0].size() - 1;
        while(true) {
            for(int i=left; i<=right; i++) result.push_back(matrix[up][i]);
            if(++up > down) break;
            
            for(int i=up; i<=down; i++) result.push_back(matrix[i][right]);
            if(--right<left) break;

            for(int i=right; i>=left; i--) result.push_back(matrix[down][i]);
            if(--down<up) break;

            for(int i=down; i>=up; i--) result.push_back(matrix[i][left]);
            if(++left>right) break;
        }

        return result;
    }
};
```

对于这种螺旋遍历的方法，重要的是要确定上下左右四条边的位置，那么初始化的时候，上边up就是0，下边down就是`matrix.size() - 1`，左边left是0，右边right是`matrix[0].size() - 1`。然后我们进行while循环，先遍历上边，将所有元素加入结果result，然后上边下移一位，如果此时上边大于下边，说明此时已经遍历完成了，直接break。