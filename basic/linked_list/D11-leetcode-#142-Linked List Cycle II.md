**题目:**

142. Linked List Cycle II

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. **Note that** ```pos``` **is not passed as a parameter**.

**Notice** that you **should not modify** the linked list.

**Example 1:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)
```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```
**Example 2:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)
```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```
**Example 3:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)
```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```
**Constraints:**
- The number of the nodes in the list is in the range ```[0, 104]```.
- ```-105 <= Node.val <= 105```
- ```pos``` is ```-1``` or a **valid** index in the linked-list.

**Follow up:** Can you solve it using ```O(1)``` (i.e. constant) memory?

**Labels:**
- 中等
- 链表

**思路:**
利用两个指针slow、fast，快(fast)指针走两步，慢(slow)指针走一步。

设起点到环的入口距离为a，环的入口到相遇点距离为b，环的另一半距离为c。相遇时慢指针走的距离为a + (b + c)*m + b，快指针走的距离为两倍 2(a + (b + c)*m + b) = a + (b + c)*n + b，即head指针到入口点的距离等于slow剩余到入口处的距离 + k cycle(绕k圈)，意味着两个指针同时从head处和fast/slow相遇处出发,一定会在cycle的入口处相遇。

具体推导步骤如下:
![Aaron Swartz](https://github.com/falconruo/91alg-2/blob/main/img/leetcode-142.png)
```
when met:
slow: a + (b + c)*m + b, m为慢指针走的cycle的圈数
fast: a + (b + c)*n + b, n为快指针走的cycle的圈数
fast = 2 * slow

2*(a + b + (b + c) * m) = a + b + (b + c) * n
a + b = (b + c) *(n - 2m)
a = (b + c) * (n - 2m) - b
a = (b + c) * (n - 2m) - (b + c) + c = (b + c) * (n - 2m - 1) + c

b + c = 1 cycle

a = k cycles + c

结论: 当fast，slow相遇后，让fast指向head, slow继续指向相遇处，每次两个指针都各走一步，等fast和slow再次相遇，即为环的入口。
```

**复杂度分析:**
1. 时间复杂度: O(n * k) = O(n), n为结点数, k为可能的圈数
2. 空间复杂度: O(1)
3. 执行结果：通过
    - 执行用时：4 ms, 在所有 C++ 提交中击败了99.05%的用户
    - 内存消耗：8 MB, 在所有 C++ 提交中击败了17.46%的用户

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
    ListNode *detectCycle(ListNode *head) {
        if (!head || !head->next) return nullptr;

        /* use two pointers: 
            slow pointer -> one node each time
            fast pointer -> two nodes each time

            when meet means a cycle found
        */
        ListNode* fast = head;
        ListNode* slow = head;

        while (fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
            // has a cycle
            if (fast == slow) {
                fast = head;
                // start from head, when meet again, the meet node is the cycle begins
                while (fast != slow) {
                    fast = fast->next;
                    slow = slow->next;
                }

                return fast;
            }
        }
        return nullptr;
    }
};
```
