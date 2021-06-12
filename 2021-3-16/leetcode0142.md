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

设链表中环外部分的长度为 a。`slow` 指针进入环后，又走了 `b` 的距离与 `fast` 相遇。慢指针走过的总距离` a+b`。此时，`fast` 指针已经走完了环的 `n` 圈，因此它走过的总距离为 `a+n(b+c)+b=a+(n+1)b+nc`。

所以：`a+(n+1)b+nc=2(a+b) ⟹ a=c+(n−1)(b+c)`

![tu](https://assets.leetcode-cn.com/solution-static/142/142_fig1.png)

> 解释：为何慢指针第一圈走不完一定会和快指针相遇： 首先，第一步，快指针先进入环 第二步：当慢指针刚到达环的入口时，快指针此时在环中的某个位置(也可能此时相遇) 第三步：设此时快指针和慢指针距离为x，若在第二步相遇，则x = 0； 第四步：设环的周长为n，那么看成快指针追赶慢指针，需要追赶n-x； 第五步：快指针每次都追赶慢指针1个单位，设慢指针速度1/s，快指针2/s，那么追赶需要(n-x)s 第六步：在n-x秒内，慢指针走了n-x单位，因为x>=0，则慢指针走的路程小于等于n，即走不完一圈就和快指针相遇