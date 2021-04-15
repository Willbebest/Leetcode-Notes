####  [K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

****

**题目描述**

给你一个链表，每 *k* 个节点一组进行翻转，请你返回翻转后的链表。

*k* 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 *k* 的整数倍，那么请将最后剩余的节点保持原有顺序。

**进阶：**

- 你可以设计一个只使用常数额外空间的算法来解决此问题吗？
- **你不能只是单纯的改变节点内部的值**，而是需要实际进行节点交换。

示例 1：

![示例 1](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

示例 2：

![示例 2](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

**提示：**

- 列表中节点的数量在范围 `sz` 内
- `1 <= sz <= 5000`
- `0 <= Node.val <= 1000`
- `1 <= k <= sz`

****

**题目分析**

此题与反转链表的的思路类似，将链表的节点分成 k 个一组，满足 k 个节点就进行翻转，如果不满足，就不进行操作。

***

**我的答案**

```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* pre = new ListNode(-1, head); // 添加哨兵节点
        int counter = 0;   // 计算链表节点数量
        while(head) {
            head=head->next;
            counter ++;
        }

        head = pre;
        ListNode* cur = NULL;  // 记录当前节点
        while(counter >= k) {  // 满足 k 个节点进行翻转
            cur =pre->next;
            ListNode* back = cur;  // 标记反转后k个节点中的尾部节点
            for(int i=0; i<k; i++) {
                ListNode* node = cur;
                cur = cur->next;
                node->next = pre->next;
                pre->next = node; 
            }
            back->next = cur; // 注意 防止循环，指向正确节点
            pre =back;
            counter -= k;  // 计算剩余节点数
        }

        return head->next;
    }
};
```

