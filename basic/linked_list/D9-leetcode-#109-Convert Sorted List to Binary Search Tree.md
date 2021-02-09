**题目:**

109. Convert Sorted List to Binary Search Tree
Given the head of a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

**Example 1:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

```
Input: head = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.
```

**Example 2:**
```
Input: head = []
Output: []
```
**Example 3:**
```
Input: head = [0]
Output: [0]
```

**Example 4:**
```
Input: head = [1,3]
Output: [3,1]
```

**Constraints:**
- The number of nodes in ```head``` is in the range ```[0, 2 * 104]```.
- ```-10^5 <= Node.val <= 10^5```

**Labels:**
- 简单 Easy
- 链表 Linked List
- 双指针 Two pointers

**思路:**

因为是升序递增的linked list, 使用快慢指针的方法每次取中间结点创建树的root结点,利用递归思路再分别处理左右子树(head -> slow, slow->next -> tail)

**复杂度分析:**
1. 时间复杂度: O(nlogn), n为给定链表长度
2. 空间复杂度: O(logn), 递归栈
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
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        return createBST(head, nullptr);
    }
private:
    TreeNode* createBST(ListNode* head, ListNode* tail) {
        if (head == tail) return nullptr;

        // find the middle node between head and tail
        ListNode* fast = head;
        ListNode* slow = head;

        while (fast != tail && fast->next != tail) {
            fast = fast->next->next;
            slow = slow->next;
        }

        TreeNode* node = new TreeNode(slow->val);

        node->left = createBST(head, slow);
        node->right = createBST(slow->next, tail);

        return node;
    }
};
```
