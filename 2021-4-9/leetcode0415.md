####  [字符串相加](https://leetcode-cn.com/problems/add-strings/)

***

**题目分析**

两个数字以索引最大的字符为数字的最低位，有点类似于大端存储，所以从索引最大位开始相加。

为了节省空间以最长字符串存储返回字符串。

***

**我的答案**

```
string addStrings(string num1, string num2) {
    int len1 = num1.size();
    int len2 = num2.size();
    if(len1<len2) return addStrings(num2, num1);

    int sum =0;
    int i= len1 - 1, j= len2 - 1;
    while(i>=0) {
        sum += num1[i] -'0';
        sum += (j >= 0 ? num2[j--] - '0' : 0);
        num1[i--] = char(sum % 10+'0');  // 注意加'0'
        sum /= 10;
    }
    return sum ==0 ? num1 : string("1") + num1;// 需要注意位数溢出的情况
}
```

时间复杂度为O(n)，空间复杂度为O(1); 

程序逻辑：

> - 首先计算出两个字符串的长度。为了使用最长字符串保存返回值，这里保证 `num1`是最长字符串
> - 使用 sum 记录同位数字的相加之和，同时也记录进位
> - 当`num2`的索引移动到小于零处，默认进行加 1 。`sum/=10`是为了保存进位
> - 最后判断s是否存在遍历完`sum1`所有字符后，存在进位 1 的情况。如果存在需要加 1 

一种个简洁的写法

```cpp
string addStrings(string num1, string num2) {
    int i = num1.size() - 1;
    int j = num2.size() - 1;
    int sum = 0;
    string result;
    while(i >= 0 || j >= 0 || sum != 0) {
        sum += (i>=0 ? num1[i--]-'0':0);
        sum += (j>=0 ? num2[j--]-'0':0);
        result = char(sum%10 + '0') + result;
        sum /= 10;
    }

    return result;
    
}
```

但是占用空间更大，时间更多。因为string 底层为了添加数据频繁分配和释放空间。以及插入数据占用时间

再优化，在尾部插入

```cpp
string addStrings(string num1, string num2) {
    int i = num1.size() - 1;
    int j = num2.size() - 1;
    int sum = 0;
    string result;
    while (i >= 0 || j >= 0 || sum != 0) {
        sum += (i >= 0 ? num1[i--] - '0' : 0);
        sum += (j >= 0 ? num2[j--] - '0' : 0);
        result.push_back(char(sum % 10 + '0'));
        sum /= 10;
    }
    reverse(result.begin(), result.end());

    return result;
}
```

