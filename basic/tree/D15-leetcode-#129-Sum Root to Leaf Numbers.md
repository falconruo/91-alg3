**题目:**

129. Sum Root to Leaf Numbers
Given a binary tree containing digits from ```0-9``` only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path ```1->2->3``` which represents the number ```123```.

Find the total sum of all root-to-leaf numbers.

Note: A leaf is a node with no children.

**Example 1:**
```
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```
**Example 2:**
```
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

**Labels:**
- 中等
- 树

**思路:**

递归法（DFS）:前面值乘十加上现在节点的值再分别加上左右节点的值并返回

从根节点出发递归进行DFS遍历，使用辅助函数dfs，每次传入上一层的num
1. 如果当前节点为空，返回0
2. 如果当前节点不为空,计算当前节点的num(传入的上一层的num * 10 + node->val)
    - 如果当前节点是叶节点(leaf,左右子节点都为空), 则找到一个有效的number, 返回该num
    - 不是叶节点，则继续向下查找左右子节点, 最终返回左右子树的有效number的和

迭代法（BFS):
使用栈或队列存放左右子节点, 定义变量sum为leaf number的和。

每次将队列头节点出队，判断其是左右子节点是否为空
- 左子节点不为空,则修改其值为父节点的值乘10加上当前节点的值，并入队
- 右子节点不为空,则修改其值为父节点的值乘10加上当前节点的值，并入队
- 左右子节点都为空(leaf node)则将当前节点值加上sum

**复杂度分析:**
1. 时间复杂度: O(n), n为树的节点数
2. 空间复杂度: DFS -> O(logn), 树的深度; BFS -> O(n)
3. DFS 执行结果：通过
    - 执行用时：４ ms, 在所有 C++ 提交中击败了34.12%的用户
    - 内存消耗：８.９ MB, 在所有 C++ 提交中击败了20.65%的用户
4. BFS 执行结果：通过

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

// 递归法（DFS）
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        return dfs(root, 0);
    }
private:
    int dfs(TreeNode* node, int num) {
        if (!node) return 0;

        num = num * 10 + node->val;

        if (!node->left && !node->right)
            return num;
        
        return dfs(node->left, num) + dfs(node->right, num);
    }
};

// 迭代法（BFS)
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        if (!root) return 0;

        queue<TreeNode*> q;
        q.push(root);

        int sum = 0;

        while (!q.empty()) {
            int size = q.size();
            while (size--) {
                TreeNode* cur = q.front();
                q.pop();
                if (cur->left) {
                    cur->left->val += cur->val*10;
                    q.push(cur->left);
                }
                
                if (cur->right) {
                    cur->right->val += cur->val*10;
                    q.push(cur->right);
                }
                
                if (!cur->left && !cur->right)
                    sum += cur->val;
            }
        }
        return sum;
    }
};
```