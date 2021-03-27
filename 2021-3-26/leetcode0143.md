####  [重排链表](https://leetcode-cn.com/problems/reorder-list/)

***

**题目描述**

给定一个单链表 *L*：L<sub>0</sub>→L<sub>1</sub>→…→L<sub>n-1</sub>→L<sub>n</sub> 

将其重新排列后变为： L<sub>0</sub>→*L<sub>n</sub>*→*L*<sub>1</sub>→*L*<sub>n-1</sub>→*L*<sub>2</sub>→*L*<sub>n-2</sub>→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例 1:
```
给定链表 1->2->3->4, 重新排列为 1->4->2->3.
```

**示例 2:**

```
给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.
```

***

**题意解析**

本题需要注意点是

- 链表不能随机读取，需要首位配对拼接
- 返回值使用参数返回

***

**我的答案**

```cpp
class Solution {
public:
    void reorderList(ListNode* head) {
        deque<ListNode*> Q;
        while(head) {
            Q.push_back(head);
            head=head->next;
        }
        ListNode*node =NULL;
        head = Q.front(); Q.pop_front();
        ListNode* cur = head;
        while(Q.size()) {
            node = Q.back(); Q.pop_back();
            cur->next = node;
            cur =cur->next;
            if(Q.empty()) break;
            node = Q.front(); Q.pop_front();
            cur->next = node;
            cur =cur->next;
        }
        cur->next=NULL;
    }
};
```

时间复杂度O(n)，空间复杂度O(n)

根据题意，使用的最笨的方法：使用双端队列`deque`，先将所有节点顺序加入到`deque`中，然后依次从`deque`的收尾取出一个节点进行拼接。本以为会很慢，没想到时间超过 90%，但是空间超过37%，占空间较多。

***

**递归法**（来自网上）

```
class Solution {
public:
    void reorderList(ListNode* head) {
        if (!head || !head->next) return;
        ListNode *p = head;
        ListNode *pre = NULL;
        while (p->next) { // 寻找最后一个节点
            pre = p;
            p = p->next;   
        }
        pre->next = NULL;
        ListNode *next = head->next;
        reorderList(next); // 再次执行去掉收尾节点的链表
        
        head->next = p;
        p->next = next;
    }
};
```

时间复杂度为O($n^2$)

***

**查找中点+反转链表+合并链表**

```cpp
class Solution {
public:
    void reorderList(ListNode* head) {
        if(head==NULL||head->next==NULL) return;
        ListNode* mid = GetMidNode(head);
        ListNode* l1 =head;
        ListNode* l2 =mid;
        l2 = Reverselist(l2);
        head = MergeList(l1, l2); 
    }
	// 获取链表中间结点
    ListNode* GetMidNode(ListNode* head)
    {
        ListNode* slow = head;
        ListNode* fast = head;
        ListNode* pre = slow;
        while(fast&&fast->next) {
            pre =slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        pre->next = NULL; // 直接将原来的链表一分为二
        return slow;
    }
	// 反转链表
    ListNode* Reverselist(ListNode*head)
    {
        if(head==NULL || head->next==NULL) return head;
        head = new ListNode(-1, head);
        ListNode* pCur = head->next;
        head->next = NULL;
        while(pCur) {
            ListNode* node = pCur;
            pCur = pCur->next;

            node->next = head->next;
            head->next= node;
        }
        return head->next;
    }
	// 合并链表
    ListNode* MergeList(ListNode* l1, ListNode* l2)
    {
        ListNode *head= new ListNode(-1);
        ListNode *pCur= head;
        while(l1&&l2) {
            pCur->next = l1;
            l1=l1->next;
            pCur->next->next = l2;
            l2=l2->next;
            pCur = pCur->next->next;
        }
        pCur->next = l1?l1:l2;
        return head->next;
    }
};
```

先查找整个链表的中点mid，将链表分为l<sub>1</sub>和l<sub>2</sub>，l<sub>1</sub>代表mid之前的节点，l<sub>2</sub>代表mid之后的节点。反转l<sub>2</sub>，合并两个节点。时间复杂度为O(n)，空间复杂度为O(1)

