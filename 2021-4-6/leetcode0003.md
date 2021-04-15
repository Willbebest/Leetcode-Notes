####  [无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

***

**题目描述**

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**提示：**

- `0 <= s.length <= 5 * 104`
- `s` 由英文字母、数字、符号和空格组成

***

**题意分析**

需要找出的结果是：不含有重复字符的 **最长子串**的长度

字符串的长度可能为零

***

**我的答案**

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        size_t result = 0;
        unordered_set<char> window;
        int leftPos = 0;
        int len =  s.size();
        for(int i=0; i<len; i++) {
            while(window.count(s[i])) {
                window.erase(s[leftPos++]);
            }
            window.insert(s[i]);
            result = max(result, window.size()); // 注意此行与上一行的位置
            // result = max(result, i-start+1);
        } 

        return result;
    }
};
```

时间复杂度为 O(n)，空间复杂度为O(n)

思路来自网络题解：使用滑动窗口的方法。使用容器记录串口内容，同时使用一个标记为记录窗口左边位置。一次遍历字符串s，并记录窗口内容，当存在重复字符时，会删除包括重复字符在内的左侧所有字符。

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> charMap;
        int start = -1;
        int result = 0;
        for(int i=0; i< s.size(); i++) {
            if(charMap.count(s[i])) {
                start = max(start, charMap[s[i]]);
            } 
            charMap[s[i]] = i;
            result = max(result, i- start);
        }

        return  result;
    }
};
```

时间复杂度为 O(n)，空间复杂度为O(n)。但是实际运行时间和空间占有更小一些

使用 start 记录当前无重复子串的的起始位的前一位。

使用result记录最大无重复字符串的最大长度

使用`charMap` 记录遍历过的字符的位置的最大值。