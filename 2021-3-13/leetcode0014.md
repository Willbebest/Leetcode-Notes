#### [最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

- - -

**题目描述**
编写一个函数来查找字符串数组中的最长公共前缀。  
如果不存在公共前缀，返回空字符串 `""`。  

示例 1：  

```
输入：strs = ["flower","flow","flight"]
输出："fl"
```

示例 2：  
```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

提示：  
>- `0 <= strs.length <= 200`
>- `0 <= strs[i].length <= 200`
>- `strs[i]` 仅由小写英文字母组成

- - -

**题意分析**
求 strs 的最长公共前缀代表计算 strs 中所有元素的最长公共前缀，也就是说 strs 的最大值和最小值的公共前缀。
需要注意 strs 为空的情况

- - -

**我的解法**
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()) return string();
        auto min_iter = min_element(strs.begin(), strs.end());
        auto max_iter = max_element(strs.begin(), strs.end());
        int len = min_iter->size();
        for(int i=0; i< len; i++)  {
            if(min_iter->at(i) != max_iter->at(i)) {
                return min_iter->substr(0, i);
            }
        }
        return *min_iter;
    }
};

```
使用库算法`min_element`和`max_element`，获取 strs 中最小值和最大值，通过获取两者之间的最大公共前缀，来确定整个 strs 的最大公共前缀。需要遍历两次strs同时需要遍历一次最小element

- - -
优化解法
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()) return string();
        auto iter = minmax_element(strs.begin(), strs.end());
        int len = iter.first->size();
        for(int i=0; i< len; i++)  {
            if(iter.first->at(i) != iter.second->at(i)) {
                return iter.first->substr(0, i);
            }
        }
        return *iter.first;
    }
};
使用库算法`minmax_element`只遍历一次strs同时获取strs的最大值和最小值，减少一次遍历strs的时间
```