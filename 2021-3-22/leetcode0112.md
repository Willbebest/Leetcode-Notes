####  [路径总和](https://leetcode-cn.com/problems/path-sum/)

***

**题目描述**

给你二叉树的根节点 root 和一个表示目标和的整数 `targetSum `，判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 `targetSum` 。

![示例1](https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
```

示例 2：

![示例2](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
输入：root = [1,2,3], targetSum = 5
输出：false
```

示例 3：

```
输入：root = [1,2], targetSum = 0
输出：false
```

提示：

- 树中节点的数目在范围 `[0, 5000]` 内
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

***

**我的答案**

```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root==NULL) return false;
        if(root->val==targetSum&&!root->left&&!root->right) 	
            return true;
        return hasPathSum(root->left, targetSum-root->val)||
                hasPathSum(root->right, targetSum-root->val);
    }
};
```

使用`由上至下`的方法.



