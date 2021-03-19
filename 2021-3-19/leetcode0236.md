####  [二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

***

**题目描述**

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

示例 1：

![示例1](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)
```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

示例 2：

![示例2](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)
```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

示例 3:

```
输入：root = [1,2], p = 1, q = 2
输出：1
```

提示：

- 树中节点数目在范围 `[2, 105]` 内。
- `-109 <= Node.val <= 109`
- 所有` Node.val `互不相同 。
- `p != q`
- `p `和 `q `均存在于给定的二叉树中。

***

**题意分析**

本题想要求最近公共子树，很明显是由下至上的解法，采用后序遍历

***

**我的解法**

```cpp
class Solution {
public:
    bool CommonAncestor(TreeNode* root, TreeNode* target) {
        if(root==NULL) return false;
        if(root==target) return true;
        return CommonAncestor(root->left, target) || CommonAncestor(root->right, target);
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root==NULL) return NULL;
        TreeNode* result = lowestCommonAncestor(root->left, p,q);
        if(result) return result;
        result = lowestCommonAncestor(root->right, p,q);
        if(result) return result;
        if(CommonAncestor(root, p)&&CommonAncestor(root, q)) {
            return root;
        }
        return NULL;
    }
};
```

自底而上，后序遍历。