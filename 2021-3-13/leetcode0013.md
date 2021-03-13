#### [罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)
- - -
**题目描述**
罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字` 2` 写做` II` ，即为两个并列的` 1`。`12` 写做 `XII` ，即为 `X` +` II` 。 `27` 写做  `XXVII`, 即为 `XX` + `V` +` II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如` 4` 不写做 `IIII`，而是` IV`。数字 `1` 在数字` 5` 的左边，所表示的数等于大数` 5` 减小数` 1` 得到的数值 - --- `4` 。同样地，数字 `9` 表示为 `IX`。这个特殊的规则只适用于以下六种情况：
- `I` 可以放在 `V` (5) 和` X` (10) 的左边，来表示 4 和 9。

- `X` 可以放在` L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 

- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

示例1：

```
输入: "III"
输出: 3
```

示例2：

```
输入: "IV"
输出: 4
```

示例 3:

```
输入: "IX"
输出: 9
```

示例 4:

```
输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```

示例 5:

```
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

**提示：**

  - 1 <= s.length <= 15
  - s 仅含字符 ('I', 'V', 'X', 'L', 'C', 'D', 'M')
  - 题目数据保证 s 是一个有效的罗马数字，且表示整数在范围 [1, 3999] 内
  - 题目所给测试用例皆符合罗马数字书写规则，不会出现跨位等情况。
  - `IL` 和 `IM` 这样的例子并不符合题目要求，49 应该写作 `XLIX`，999 应该写作 `CMXCIX` 。


- - - - -

**题意分析**

题目中的关键信息

1.  字符和数值的对应关系表
2.  罗马数字之间的摆放关系
	- 通常罗马数字中小的数字在大的数字的右边
	- 特殊情况下，
		- `I `可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
		- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。
		- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

- - -

**我的解法**
```cpp
class Solution {
public:
    int romanToInt(string s) {
        int result =0;
        // if(s.empty()) return result; // 因为题意中表明了s的size大于1
        int len = s.size() -1;
        for(int i = 0; i < len; i++) {
            if(mapping[s[i]]<mapping[s[i+1]]) { // 特殊情况
                result -= mapping[s[i]];
            } else { // 通常情况
                result += mapping[s[i]];
            }
        }
        result += mapping[s.back()];
        return result;
    }

private:
    unordered_map<char, int> mapping = {
        {'I', 1},
        {'V', 5},
        {'X', 10},
        {'L', 50},
        {'C', 100},
        {'D', 500},
        {'M', 1000}        
    };
};
```
根据题意分析，使用`unordered_map`保存罗马字符和数字之间的映射
然后遍历字符串`s`，分析字符串之间的摆放关系，来决定数字的加减。
根据前几题的经验存储for循环的上界，使用空间换时间。事件复杂度为 O(n)

- - -

**改进的写法**
```cpp
class Solution {
public:
    int romanToInt(string s) {
        int result =0;
        if(s.empty()) return result;
        int len = s.size() -1;
        for(int i = 0; i < len; i++) {
            if(getValue(s[i]) < getValue(s[i+1]) ) { // 特殊情况
                result -= getValue(s[i]);
            } else { // 通常情况
                result += getValue(s[i]);
            }
        }
        result += getValue(s.back());
        return result;
    }

private:
    int getValue(char ch) {
        switch(ch) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
            default: return 0;
        }
    }
};
```
使用函数接口封装一个switch来代替map，减少时间和空间

> 留下了一个疑问： 使用 getValue 为啥会比 使用 map 快？