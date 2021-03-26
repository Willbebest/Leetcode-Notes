#####  [字符串解码](https://leetcode-cn.com/problems/decode-string/)

***

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 *encoded_string* 正好重复 *k* 次。注意 *k* 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 *k* ，例如不会出现像 `3a` 或 `2[4]` 的输入。

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

**示例 2：**

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```

**示例 3：**

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

**示例 4：**

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

***

**解法一：使用双栈**

```cpp
class Solution {
public:
    string decodeString(string s) {
        string result;
        stack<int> number;
        stack<string> strs;
        int multi = 0;
        for(auto c : s) {
            if(isdigit(c)) { // 数字
                multi = multi*10 + c - '0';
            } else if(c == '[') { // 新的子串开始
                strs.push(result);
                number.push(multi);
                multi = 0;
                result.clear();
            } else if(isalpha(c)) { // 字符
                result += c;
            } else {              // 子串结束
                string top = strs.top(); strs.pop();
                int num = number.top(); number.pop();
                while(num--) top += result;
                result = top;
            }
        }
        return result;
    }
};
```

使用两个栈分别存储子串和数字（个数），对 s 依次遍历，字符 c 可能有四种情况：

-  当字符是一个数字时，存储数字到multi中，multi表示子串的个数。之所以使用`multi = multi*10 + c - '0'`是因为淄川的个数可能超过10，有多位字符表示
- 当字符是一个`'['`，表示新的子串的开始。需要将当前子串result和个数multi添加到各自栈中进行记录。然后开始记录子串的情况
- 当字符仅仅是一个字符时，添加字符到result中，进行记录，result记录当前值字符串
- 当字符是`']'`，表示子串的结束。需要根据之前的子串和当前子串的个数进行拼接。

***

**递归法**

```cpp
class Solution {
public:
    string decodeString(string s) {
        int i =0;
        return dfs(s, i);
    }

    string dfs(string s, int& i) {
        string result;
        int multi = 0;
        while(i < s.size()) {
            if(isdigit(s[i])) {  // 数字
                multi = multi * 10 + int(s[i] - '0');
            } else if(s[i] == '[') { // 子串的开始
                string temp = dfs(s, ++i);
                while(multi) {
                    result += temp;
                    multi--;
                }
            } else if(s[i] == ']') {  // 子串的结束
                return result;
            } else {
                result += s[i]; // 积累子串
            }
            i++;
        }
        return result;
    }
};
```

使用递归来替换栈，基本思想与双栈的类似，也是依次遍历子串，字符四种：

-  当字符是一个数字时，存储数字到multi中，multi表示子串的个数。之所以使用`multi = multi*10 + c - '0'`是因为子串的个数可能超过10，可能由多位字符表示
-  当字符是一个`'['`，表示新的子串的开始。使用`dfs`计算子串。
-  当字符仅仅是一个字符时，添加字符到result中，进行记录，result记录当前值子串
-  当字符是`']'`，表示子串的结束。返回结果。