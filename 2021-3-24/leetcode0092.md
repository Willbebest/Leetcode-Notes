####  [反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

***

**题目描述**

给你单链表的头指针 `head` 和两个整数 `left` 和 `right` ，其中 `left <= right` 。请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。

![leetcode92](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

**示例 2：**

```
输入：head = [5], left = 1, right = 1
输出：[5]
```

**提示：**

- 链表中节点数目为 `n`
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

**进阶：** 你可以使用一趟扫描完成反转吗？

***

**我的解法**

```
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        int n = right - left;
        ListNode* temp =head;
        for(int i=1; i<left -1; i++) {
            temp = temp->next;
        }
        ListNode* node =NULL;
        ListNode* pCur = temp->next;
        for(int i = 0; i<n; i++) {
            node = pCur->next;
            pCur->next = pCur->next->next;

            node->next = temp->next;
            temp->next =node;  
        }
        return head;
    }
};
```

需要反转的地方，使用头插法

需要注意的是：这里的left和right不是索引，而是从 1 开始的第几个。