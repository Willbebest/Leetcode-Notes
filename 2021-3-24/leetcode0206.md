####  [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

***

反转一个单链表。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**进阶:**
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

***

**我的解法**

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = head;
        head = NULL;
        while(cur) {
            ListNode* node = cur;
            cur = cur->next;

            node->next = head;
            head = node;
        }
        return head;
    }
};
```

使用头插法，依次遍历并插入。

递归法

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL||head->next==NULL) return head;
        ListNode* result=reverseList(head->next);
        head->next->next=head;
        head->next=NULL;
        return  result;
    }
};
```

