**题目:**

24. Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head.

You may not modify the values in the list's nodes. Only nodes itself may be changed.

**Example 1:**
![Aaron Swartz](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)
```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

**Example 2:**
```
Input: head = []
Output: []
```

**Example 3:**
```
Input: head = [1]
Output: [1]
```

**Constraints:**
- The number of nodes in the list is in the range [0, 100].
- ```0 <= Node.val <= 100```

**Labels:**
- 简单
- 链表

**思路:**
方法一、
1. 对于空链表或者只有一个结点的链表，直接返回head
2. 因为结点互换，head指向的结点会变化，所以定义一个dummy结点，令其指向给定链表的头结点(dummy.next = head), 然后令head指向dummy结点(head = &dummy)，则dunny.next指向的结点即为新的链表的头结点, 新的链表如下：
    - dummy_0->node_1->node_2->...->node_n
3. 对于两个以上结点的链表，循环遍历整个链表,终止条件head->next->next不存在
    - 定义两个辅助指针：p1指向head->next，p2指向head->next->next
    - 两两交换p1和p2指向的结点, 并且让head->next指向p2, 完成后移动head到p1指向的新的结点
4. 最后返回dummy.next

方法二、
递归法
1. 每两个节点两两交换，head为原始链表头节点，p为head的下一个节点
2. 交换原则: p->next指向head节点，head->next指向后续已经交换好的链表(swapPairs(p->next)), 其中起始节点为p->next.
3. 交换后的新链表的起始节点为p，最后返回p

**复杂度分析:**
1. 时间复杂度: O(n), n为给定链表长度
2. 空间复杂度: O(1)
3. 方法一、执行结果：通过
    - 执行用时：4 ms, 在所有 C++ 提交中击败了73.67%的用户
    - 内存消耗：7.3 MB, 在所有 C++ 提交中击败了93.99%的用户
4. 方法二、执行结果：通过
    - 执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户
    - 内存消耗：7.3 MB, 在所有 C++ 提交中击败了95.34%的用户

**代码(C++):**
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (!head || !head->next) return head;

        ListNode* dummy = new ListNode(0);
        dummy->next = head;

        head = dummy;

        while (head && head->next && head->next->next) {
            auto p1 = head->next;
            auto p2 = head->next->next;

            p1->next = p2->next;
            p2->next = p1;

            head->next = p2;
            head = p1;
        }

        return dummy->next;
    }
};


递归法
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (!head || !head->next) return head;

        ListNode* p = head->next;
        head->next = swapPairs(p->next);
        p->next = head;
        return p;
    }
};
```
