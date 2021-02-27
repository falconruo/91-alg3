**题目:**

513. Find Bottom Left Tree Value
Given a binary tree, find the leftmost value in the last row of the tree.

**Example 1:**
```
Input:

    2
   / \
  1   3

Output:
1
```
**Example 2:**
```
Input:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

Output:
7
```
Note: You may assume the tree (i.e., the given root node) is not **NULL**.

**Labels:**
- 树 Tree
- 中等 Medium

**思路:**

递归法（DFS):

从根节点出发递归进行DFS遍历，用变量maxdep记录当前节点深度, 开始设置leftmost节点的值=root->val,使用辅助函数dfs
1. 如果当前节点为叶子节点并且节点深度dep > maxdep, 将maxdep++并且设置leftmost = node->val, 此时必然为当前层(dep)的最左边的节点,返回
2. 如果当前节点的左子节点不为空,递归调用dfs, 使节点(node->left)的dep + 1
3. 如果当前节点的右子节点不为空,递归调用dfs, 使节点(node->right)的dep + 1

迭代法（BFS):
层次遍历, 使用队列存放节点

1. 将根节点root入队
2. 循环判断队列是否空
    - 令leftmost等于队列头节点的值，因为按层序遍历,每次的队头肯定是当前层的最左边节点
    - 取当前队列size(树的当前层的节点数), 循环将同一层每一个节点依次出队,并且把不为空的子节点插入入队尾

**复杂度分析:**
1. 时间复杂度: O(n), n为树的节点数
2. 空间复杂度: DFS -> O(logn), 树的深度; BFS -> O(k), k为最宽一层的节点数
3. DFS 执行结果：通过
    - 执行用时：16 ms, 在所有 C++ 提交中击败了73.05%的用户
    - 内存消耗：21.1 MB, 在所有 C++ 提交中击败了94.50%的用户
4. BFS 执行结果：通过
    - 执行用时：20 ms, 在所有 C++ 提交中击败了45.44%的用户
    - 内存消耗：21.3 MB, 在所有 C++ 提交中击败了66.22%的用户

**代码(C++):**
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

/**
 * DFS + Recursive
 */
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        leftmost = root->val;
        maxdep = 1;
        dfs(root, 1);

        return leftmost;
    }
private:
    int maxdep;
    int leftmost;
    void dfs(TreeNode* node, int depth) {
        if (!node->left && !node->right) {
            if (depth > maxdep) {
                leftmost = node->val;
                maxdep = depth;
            }
            return;
        }

        if (node->left)
            dfs(node->left, depth + 1);
        
        if (node->right)
            dfs(node->right, depth + 1);
    }
};

/**
 * BFS + Iterative
 */
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> qt;
        qt.push(root);
        int res;

        while (!qt.empty()) {
            TreeNode* node = qt.front();
            res = node->val;

            size_t s = qt.size();
            while (s--) {
                qt.pop();

                if (node->left)
                    qt.push(node->left);

                if (node->right)
                    qt.push(node->right);
                
                node = qt.front();
            }
        }
        return res;
    }
};
```
