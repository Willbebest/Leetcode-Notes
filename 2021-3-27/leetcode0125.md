####  [验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

****

**题目描述**

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串

**示例 1:**

```
输入: "A man, a plan, a canal: Panama"
输出: true
```

**示例 2:**

```
输入: "race a car"
输出: false
```

****

**我的答案**

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int left = 0; 
        int right = s.size() - 1;
        while(left < right) {
            while(left<right&&!isalnum(s[left])) left++;
            while(left<right&&!isalnum(s[right])) right--;
            if(tolower(s[left++])!=tolower(s[right--])) {
                return false;
            } 
        }
        return true;
    }
};
```

时间复杂度 O(n)，空间复杂度O(1)