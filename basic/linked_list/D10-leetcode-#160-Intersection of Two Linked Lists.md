**题目:**

160. Intersection of Two Linked Lists
Write a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:
![Aaron Swartz](https://assets.leetcode.com/uploads/2018/12/13/160_statement.png)

begin to intersect at node c1.

**Example 1:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2020/06/29/160_example_1_1.png)
```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Reference of the node with value = 8
Input Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```
**Example 2:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2020/06/29/160_example_2.png)
```
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Reference of the node with value = 2
Input Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```
**Example 3:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)
```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: null
Input Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```
**Notes:**
- If the two linked lists have no intersection at all, return ```null```.
- The linked lists must retain their original structure after the function returns.
- You may assume there are no cycles anywhere in the entire linked structure.
- Each value on each linked list is in the range ```[1, 10^9]```.
- Your code should preferably run in O(n) time and use only O(1) memory.

**Labels:**
- 简单
- 链表

**思路:**
比较两个链表是否有交叉结点，定义两个指针p, q分别指向headA, headB
1. 首先计算每个链表的结点个数，存入变量lenA, lenB，如果任意一个链表为空则无交叉，返回nullptr
2. 令p指向结点数多的链表, q指向结点数少的链表, 令两个链表的长度差为diff
    - 首先移动长的链表直至diff为0
    - 同时移动两个链表，并判断结点是否相等，相等则说明是交叉结点并返回当前结点，不等则继续指向下一个结点，直至表尾
3. 如果没有交叉(intersection)返回nullptr

**复杂度分析:**
1. 时间复杂度: O(n + m), n, m为给定链表A和B长度
2. 空间复杂度: O(1)
3. 执行结果：通过
    - 执行用时：28 ms, 在所有 C++ 提交中击败了97.28%的用户
    - 内存消耗：33.2 MB, 在所有 C++ 提交中击败了5.4%的用户

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lenA = 0;
        int lenB = 0;

        ListNode* p = headA;
        ListNode* q = headB;

        while (p) {
            p = p->next;
            ++lenA;
        }

        while (q) {
            q = q->next;
            ++lenB;
        } 
        
        if (lenA == 0 || lenB == 0) return nullptr;

        p = lenA >= lenB ? headA : headB;
        q = lenA < lenB ? headA : headB;

        int diff = abs(lenA - lenB);
        while (p && diff--)
            p = p->next;
        
        while (p && q) {
            if (p == q)
                return p;
            
            p = p->next;
            q = q->next;
        }

        return nullptr;
    }
};
```
**思路:**
![Aaron Swartz](https://github.com/falconruo/91alg-3/blob/main/image/leetcode-160)

利用双指针，两个指针分别指向两个链表的头节点，此时有两种情况：

1. 链表A的长度和链表B的长度相等，它们每次都走一步，在遍历完链表前，肯定会在相交点相遇
2. 链表A的长度和链表B的长度不相等
这里主要是第2种情况的处理麻烦，当链表A和链表B长度不相等时，如果同时从头节点遍历不可能会相交，所以需要消除两个链表的长度差

消除长度差（假设B链表比A链表长）

- 指针 pA 指向 A 链表，指针 pB 指向 B 链表，依次往后遍历
- 如果 pA 遍历完链表A了 为null时，下一步让它从B链表头节点开始，即 pA = headB 继续遍历；(此时pB指针到B链表末尾是A链表和B链表的长度差)
- 如果 pB 遍历完链表B了 为null时，下一步让它从A链表头节点开始，即 pB = headA 继续遍历；(此时pB走完了B链表，而pA走完了长度差的长度，pB指针回到了A链表头节点，即相当于pA和pB消除了长度差，此时距离各自的链表尾是同样的长度，同时走如果相交肯定会相遇)

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *pA = headA, *pB = headB;
        while(pA != pB){
            pA = pA ? pA->next : headB;
            pB = pB ? pB->next : headA;
        }
        return pA;
    }
};
```
