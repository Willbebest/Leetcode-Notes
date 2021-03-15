#### [二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
***

**题目描述**
给定一个二叉树，返回其节点值的锯齿形层序遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。  
例如：

给定二叉树`[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层序遍历如下：

```
[
  [3],
  [20,9],
  [15,7]
]
```

***

**题意分析**
此题类似于二叉树的层次遍历，但是与二叉树的层次遍历不同的是：从上至下，奇数层从左到右输出，偶数层从右向左输出。

***

**我的解法**
```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        if(root==NULL) return result;
        queue<TreeNode*> Q;
        Q.push(root);
        TreeNode* node = NULL;
        while(Q.size()) {
            vector<int> level;
            size_t len = Q.size();
            for(int i=0; i<len; i++) {
                node = Q.front();
                Q.pop();
                level.push_back(node->val);
                if(node->left) Q.push(node->left);
                if(node->right) Q.push(node->right);
            }
            if(result.size()%2) reverse(level.begin(), level.end());
            result.push_back(level);
        }
        return result;
    }
};
```

相比层次遍历多在一个翻转vector。

![BFS](https://pic.leetcode-cn.com/1608599340-TRBfIV-image.png "图片来自网络")

***

**DFS解法**

```cpp
class Solution {
public:
    void dfs(TreeNode *root, int level, vector<vector<int>> &result){
        if(root==NULL) return;
        if(result.size() <= level) result.push_back(vector<int>());
        if(level%2){
            result[level].insert(result[level].begin(), root->val);
        } else {
            result[level].push_back(root->val);
        }
        dfs(root->left, level+1, result);
        dfs(root->right, level+1, result);
    }
    
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        dfs(root, 0, result);
        return result;
    }
};
```

上面的代码使用的前序遍历的递归形式，也可以使用栈进行DFS遍历


