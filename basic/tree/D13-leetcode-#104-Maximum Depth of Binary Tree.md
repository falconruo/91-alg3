**题目:**

104. Maximum Depth of Binary Tree

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Note**: A leaf is a node with no children.

**Example:**

Given binary tree ```[3,9,20,null,null,15,7]```,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its depth = 3.

**Labels:**
- 简单 Easy
- 树 Tree
- 递归 Recursive
- 迭代 Iterative

**思路:**
1. 迭代(Iterative): level order遍历二叉树，使用一个队列存放每层的子节点
2. 递归(Recursive): 根节点的深度 = 左右子树的深度的最大值+1

**复杂度分析:**
1. 时间复杂度: O(n), n为树的节点数
2. 空间复杂度: O(n), n为递归栈空间; 迭代时队列空间最大空间O(k), k为单层最大节点数
3. 执行结果：通过
Iterative:
- 执行用时：12 ms, 在所有 C++ 提交中击败了66.56%的用户
- 内存消耗：18.4 MB, 在所有 C++ 提交中击败了57.50%的用户

Recursive：
- 执行用时：8 ms, 在所有 C++ 提交中击败了91.25%的用户
- 内存消耗：19.4 MB, 在所有 C++ 提交中击败了62.12%的用户

**代码(C++):**
```C++

// Iterative
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
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        
        int dep = 0;

        queue<TreeNode*> qt;
        qt.push(root);
        
        while (!qt.empty()) {
            size_t num = qt.size();
            while (num--) {
                TreeNode* node = qt.front();
                qt.pop();
                
                if (node->left)
                    qt.push(node->left);
                if (node->right)
                    qt.push(node->right);
            }
            ++dep;
        }
        return dep;
    }
};

// Recursive
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        
        int dep = max(maxDepth(root->left), maxDepth(root->right)) + 1;
        
        return dep;
    }
};
```