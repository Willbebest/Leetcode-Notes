####  [二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

***

**题目描述**

给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

**示例:**

```cpp
输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

***

**题目分析**

想要返回每一层最左侧的元素

****

**非递归**

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        if(root==NULL) return result;
        queue<TreeNode*> Q;
        Q.push(root);
        while(Q.size()) {
            size_t len = Q.size();
            result.push_back(Q.back()->val);
            for(int i=0; i<len; i++) {
                TreeNode* node = Q.front(); Q.pop();
                if(node->left) Q.push(node->left);
                if(node->right) Q.push(node->right);
            }
        }

        return result;
    }
};
```

