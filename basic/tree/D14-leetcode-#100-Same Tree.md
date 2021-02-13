**题目:**

100. Same Tree

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

**Example 1:**
```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true
```

**Example 2:**
```
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false
```

**Example 3:**
```
Input:     1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

Output: false
```

**Labels:**
- 简单 Easy
- 树 Tree

**思路:**
1. 递归法: 先比较根节点p和q, 如果两个都为空返回true; 如果两个都不为空则分别比较值和左右子树
2. 迭代法: 用两个辅助队列

**复杂度分析:**
1. 时间复杂度: O(min(m,n)), m, n为树的节点数
2. 空间复杂度: 
    - 递归法: O(min(logm, logn)), logm/logn为树的深度, 递归栈的空间
    - 迭代法：O(min(m, n))
3. 递归法： 执行结果：通过
    - 执行用时：0 ms, 在所有 C++ 提交中击败了100%的用户
    - 内存消耗：9.7 MB, 在所有 C++ 提交中击败了85.98%的用户
4. 迭代法：执行结果：通过
    - 执行用时：0 ms, 在所有 C++ 提交中击败了100%的用户
    - 内存消耗：9.7 MB, 在所有 C++ 提交中击败了85.98%的用户

**代码(C++):**
```C++
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

1. 递归法:
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {

        if (p && q)
            return (p->val == q->val && isSameTree(p->left, q->left) && isSameTree(p->right, q->right));
        else if (!p && !q)
            return true;

        return false;
    }
};

2. 迭代法:
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        queue<TreeNode*> p1;
        queue<TreeNode*> q1;

        p1.push(p);
        q1.push(q);

        while (!p1.empty() && !q1.empty()) {
            TreeNode* node_p = p1.front();
            p1.pop();
            TreeNode* node_q = q1.front();
            q1.pop();

            if (!node_p && !node_q) continue;

            if (!node_p || !node_q || (node_p->val != node_q->val)) return false;

            p1.push(node_p->left);
            p1.push(node_p->right);
            q1.push(node_q->left);
            q1.push(node_q->right);
        }

        return true;
    }
};
```