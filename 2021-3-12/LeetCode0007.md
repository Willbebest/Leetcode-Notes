####  整数反转
- - -
__题目描述__
x 是一个 32 位的有符号整数  ，返回将 x 中的数字部分反转后的结果。
如果反转后整数超过 32 位的有符号整数的范围 [$−2^{31}$,  $2^{31}$ − 1] ，就返回 0。
**假设环境不允许存储 64 位整数（有符号或无符号）。**

**示例 1：**

```
输入：x = 123
输出：321
```

**示例 2：**

```
输入：x = -123
输出：-321
```

**示例 3：**

```
输入：x = 120
输出：21
```
- - -

**我的题解**
```cpp
int reverse(int x) {
    long long int temp = 0;
    stack<int> sti;
    while(x) {
        sti.push(x % 10);
        x /= 10;
    } 
    int length = sti.size();
    long long int base = 1;
    for(int i = 0; i < length; i++) {
        temp = temp + sti.top() * base;
        sti.pop();
        base *= 10;
    }
    return temp > INT_MAX || temp <INT_MIN ? 0 :temp;
}
```
思路：使用栈反转数字。先存储每位数字到栈中，然后再依次出栈，进行拼接。虽然通过了，但是违背了题目中的不允许使用64位整数的限制条件
- - -
**优秀解法**

```cpp
int reverse(int x) {
    long long res = 0;
    while(x) {
        res = res*10 + x%10;
        x /= 10;
    }
    return (res<INT_MIN || res>INT_MAX) ? 0 : res;
}
```
思路：只使用一次遍历， 边遍历获取每位数字，边进行相加拼接。相比上边的解法减少了一半的循环次数，并且使用的空间更小
- - -
**总结**
仔细读题，注意有符号整数的范围