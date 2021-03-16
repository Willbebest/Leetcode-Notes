#### [环形链表 II：返回入环电](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

***

**题目描述**

此题与[leetcode141](https://leetcode-cn.com/problems/linked-list-cycle/)题目描述大体一致，唯一不同就是返回值不同。如果存在环，返回值时指向入环的指针；如果不存在环，返回值为NULL。

***

**题意分析**

此题可以使用容器存储遍历过的节点，然后每遍历一个节点，查看是否纯在于容器中，如果存在，返回节点；如果不存在，添加到容器中。

进阶的解法：

***

**我的解法**

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode*> lists;
        while(head) {
            if(lists.count(head)){
                return head;
            } else {
                lists.insert(head);
                head=head->next;
            }
        }
        return NULL;
    }
};
```

使用容器hashset

***

**双指针解法(思路来自网络)**

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head==NULL) return NULL;
        ListNode* slow = head;
        ListNode* fast = head;
        while(fast&&fast->next) {
            fast=fast->next->next;
            slow=slow->next;
            if(fast==slow){
                while(head!=slow) {
                    head=head->next;
                    slow=slow->next;
                }
                return head;
            } 
        }
        return NULL;
    }
};
```

要点在于
* fast代表快指针，slow代表慢指针；快指针的速度是慢指针的2倍
* 假设快指针走过的距离是 f， 慢指针走过的距离是s。则f=2s。
* fast比slow多走了n个环长度，假设环长为b，则f=s+nb;
* 所以 f=2nb，s=nb，n表示n个环长
* 如果指针头部到换入口距离为w，那么当快慢指针相遇时，慢指针Juin入口点的距离等于 w
