####  [两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

***

**题目描述**
给你两个非空链表，代表两个非负的整数。数字最高位位于链表的开始位置。
他们的每个节点只存储一位数字。将这两个数相加会返回一个新的链表。
你可以假设除了数字 0 以外，这两个数字不会以零开头。
进阶：
如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。
示例：

```
输入：(7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 8 -> 0 -> 7
```
***

**题意分析**
两个链表代表两个两个非负的整数。整数的最高位位于链表的开始。对这样的两个链表相加相当于，从链表的末端开始依次向顶端相加。可以使用栈或者将链表翻转后相加。

***
**我的解法(参考网上思路)**
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> st1;
        stack<int> st2;
        while(l1) {
            st1.push(l1->val);
            l1=l1->next;
        }
        while(l2) {
            st2.push(l2->val);
            l2=l2->next;
        }
        int sum =0;
        ListNode* node = new ListNode(0);
        while(st1.size()||st2.size()){
            if(st1.size()) {
                sum += st1.top();
                st1.pop();
            }
            if(st2.size()) {
                sum+=st2.top();
                st2.pop();
            }
            node->val = sum%10;
            ListNode* head = new ListNode(sum/10);
            head->next = node;
            node = head;
            sum = sum/10;
        }
        return node->val==0 ? node->next:node;
    }
};
```

使用栈存储两个整数的数字。然后依次出栈相加构造新的整数结构。`ListNode* head = new ListNode(sum/10)`用于解决`99+1`的情况，`sum = sum/10`用于记录数字相加大于10的情况，结尾的`node->val==0 ? node->next:node`用于判断头结点是否为0.

***








