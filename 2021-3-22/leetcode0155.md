####  [最小栈](https://leetcode-cn.com/problems/min-stack/)

***

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

- `push(x)` —— 将元素 x 推入栈中。
- `pop()` —— 删除栈顶的元素。
- `top()` —— 获取栈顶元素。
- `getMin()` —— 检索栈中的最小元素。

**示例:**
```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

**提示：**

- `pop`、`top` 和 `getMin` 操作总是在 **非空栈** 上调用。

***

**题意分析**

本题主要时模仿写一个栈，同时需要听啊加一个属性记录栈内元素的最小值

还有一个注意的地方: `pop、top、getMin 操作总在非空栈上调用`，减少了很多麻烦

***

**我的答案**

```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
    }
    
    void push(int val) {
        if(_top==-1){
            _minVal=val;
        }else {
            _minVal = min(_minVal, val);
        }  

        if(_buffer.size()>_top+1) {
            _buffer[++_top] =val;
        } else {
            _buffer.push_back(val);
            _top++;
        }  
    }
    
    void pop() {
        if(_buffer[_top]==_minVal){
            auto iter = min_element(_buffer.begin(), _buffer.begin()+_top);
            _minVal = *iter;
        }
        _top--;
    }
    
    int top() {
        return _buffer[_top];
    }
    
    int getMin() {
        return _minVal;
    }
private:
    vector<int> _buffer{};
    int _top{-1};
    int _minVal{};
};
```

需要注意的地方主要是当栈内没有元素时，再push元素，最小值的变化。