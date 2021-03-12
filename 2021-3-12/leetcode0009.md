#### 回文数

- - -
**题目描述**
给你一个整数` x` ，如果 `x` 是一个回文整数，返回 true ；否则，返回 false 。
回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，121 是回文，而 123 不是。
示例 1：
```
输入：x = 121
输出：true
```
示例 2：
```
输入：x = -121
输出：false
解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```
示例 3：
```
输入：x = 10
输出：false
解释：从右向左读, 为 01 。因此它不是一个回文数。
```
提示：
```
-231 <= x <= 231 - 1
```
**进阶**：你能不将整数转为字符串来解决这个问题吗？
- - -
**我的题解**

解法一：

```cpp
bool isPalindrome(int x) {
    string str(to_string(x));
    return equal(str.begin(), str.end(), str.rbegin());
}
```
思路：转化为 string，利用algorithm库函数equal
效率：时间 28ms， 内存 6.2M

解法二：

```cpp
bool isPalindrome(int x) {
    if( x < 0) return false;
    int64_t rev = 0;
    int temp = x;
    while(temp) {
        rev = rev*10 + temp%10;
        temp /= 10;
    }
    return rev==x ? true : false;
}
```
思路：根据定义，先根据leetcode0007的方法翻转整数，然后比较是否相同
注意：保存x的值