**题目:**

876. Middle of the Linked List

Given a non-empty, singly linked list with head node head, return a middle node of linked list.

If there are two middle nodes, return the second middle node.

**Example 1:**
```
Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])
The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
Note that we returned a ListNode object ans, such that:
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.
```

**Example 2:**
```
Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
Since the list has two middle nodes with values 3 and 4, we return the second one.
```

**Note:**:
- The number of nodes in the given list will be between `1` and `100`.

**Labels:**
- 双指针 Double Pointers (slide-windows)
- 简单 Easy

**思路:**

快慢指针法, 指针slow每次走一步, 指针fast每次走两步, 当fast->next = null 时表示链表结尾，此时指针slow指向的结点即为中间结点

**复杂度分析:**

时间复杂度: O(n), n为链表结点个数
空间复杂度: O(1)

执行结果：通过
- 执行用时：4 ms, 在所有 C++ 提交中击败了37.45%的用户
- 内存消耗：7 MB, 在所有 C++ 提交中击败了5.15%的用户

**代码(C++):**
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        if (!head || !head->next) return head;

        ListNode* slow = head;
        ListNode* fast = head;

        while (fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
        }

        return slow;
    }
};
```