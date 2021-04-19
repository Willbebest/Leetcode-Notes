#### [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

- - -
**题目描述**
将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。
示例1:

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

示例 2：

```
输入：l1 = [], l2 = []
输出：[]
```

示例 3：

```
输入：l1 = [], l2 = [0]
输出：[0]
```

提示：
  - 两个链表的节点数目范围是 `[0, 50]`  
  - `-100 <= Node.val <= 100`  
  - `l1` 和 `l2` 均按 **非递减顺序** 排列  

* * *

**题意分析**

1. 两个链表为升序链表
2. 拼接后的链表为非递减顺序排列

* * *

**我的解法**
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1==NULL) return l2;
        if(l2==NULL) return l1;
        ListNode* head;
        if(l1->val <= l2->val) {
            head= l1;
            l1=l1->next;
        } else {
            head =l2;
            l2=l2->next;
        }
        ListNode* curNode = head;
        while(l1&&l2) {
            if(l1->val <= l2->val) {
                curNode->next= l1;
                l1=l1->next;
            } else {
                curNode->next =l2;
                l2=l2->next;
            }
            curNode = curNode->next;
        }
        if(l1) {
            curNode->next = l1;
        } else {
            curNode->next = l2;
        }
        return head;
    }
};
```

依次遍历两个链表，选出`val`最小的节点添加到目标链表中。

***

**添加头结点**

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* head =new ListNode(-1);
        ListNode* pre=head;;
        while(l1&&l2) {
            if(l1->val <= l2->val) {
                pre->next=l1;
                l1=l1->next;
            } else {
                pre->next = l2;
                l2 = l2->next;
            }
            pre = pre->next;
        }
        pre->next = (l1==NULL ? l2 : l1);

        return head->next;
    }
};
```

* * *

**递归解法(来源网上)**

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1==NULL) {
            return l2;
        } else if(l2==NULL) {
            return l1;
        } else if(l1->val <= l2->val) {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        } else {
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```

