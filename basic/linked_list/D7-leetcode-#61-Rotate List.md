**题目:**

61. Rotate List

Given a linked list, rotate the list to the right by k places, where k is non-negative.

**Example 1:**
```
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```

**Example 2:**
```
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
```

**Labels:**
- 中等 Medium
- 链表 Linked List

**思路:**

![Aaron Swartz](https://user-images.githubusercontent.com/3301667/98454001-4ffd6080-2114-11eb-9da5-d3b4d33d8501.jpeg)

将list向右旋转k步即为将list的头（head)向右移动 n - k步, 也就是新的头结点是第n - k个结点
1. 对于空链表直接返回head
2. 利用一个新的指针p,循环遍历list直至尾部(p->next为NULL),记录list的结点数n, 并连接p指向的结点与head指向的结点，形成一个cycled list, 其中指针p指向的结点为head结点的前置结点
3. 因为输入的旋转步数可能大于n, 所以令 k = k % n，则head需要向右移动的步数为n - k
4. 向右移动n - k 步，每次令p指向下一个结点，循环结束, 指针p指向的结点为新的list的头结点的前置结点
5. 令head指向p的next结点(head = p->next), 断开p指向的结点和head指向的结点 (p->next = nullptr)

**复杂度分析:**
1. 时间复杂度: O(n), n为给定链表长度
2. 空间复杂度: O(1)
3. 执行结果(C++)：通过
    - 执行用时：4 ms, 在所有 C++ 提交中击败了99.64%的用户
    - 内存消耗：7.5 MB, 在所有 C++ 提交中击败了94%的用户
4. 执行结果(Python3)：通过
    - 执行用时：52 ms, 在所有 Python3 提交中击败了18.41%的用户
    - 内存消耗：14.9 MB, 在所有 Python3 提交中击败了21.76%的用户

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
public:
    ListNode* rotateRight(ListNode* head, int k) {

        if (!head || !head->next) return head;
        int n = 1;

        ListNode* p = head;
        // get the length of list
        while (p->next) {
            p = p->next;
            ++n;
        }
        // connect to a cycled list
        p->next = head;

        int left = n - k % n;

        while (left--)
            p = p->next;

        head = p->next;
        // break the cycle
        p->next = nullptr;
        
        return head;
    }
};
```

**代码(Python3)**
```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        if not head or not head.next:
            return head

        p = head

        n = 1
        while p.next is not None:
            p = p.next
            n += 1
        
        left = n - k % n

        p.next = head

        while left > 0:
            p = p.next
            left -= 1

        head = p.next
        p.next = None

        return head
```
