#####  [相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

****

**题目描述**

编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表**：**

![图一](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

在节点 `c1` 开始相交。

示例 1：

![示例二](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

注意：

- 如果两个链表没有交点，返回 null.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

****

**我的答案**

```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB, int dis)
    {
        while(dis--) {
            headA = headA->next;
        }

        while(headA) {
            if(headA==headB) {
                return headA;
            }
            headA = headA->next;
            headB = headB->next;
        }

        return NULL;
    }

    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(headB==NULL || headA==NULL) return NULL;
        int lenA = 0;
        ListNode* temp = headA;
        while(temp) {
            lenA++;
            temp = temp->next;
        }

        int lenB = 0;
        temp = headB;
        while(temp) {
            lenB++;
            temp = temp->next;
        }

        if(lenA>lenB) return getIntersectionNode(headA, headB, lenA-lenB);
        else return getIntersectionNode(headB, headA, lenB-lenA);
    }
};
```

如果两个链表存在相交节点，那么两个相交节点距离尾部的距离一定相等。