**题目:**

987. Vertical Order Traversal of a Binary Tree

Given a binary tree, return the vertical order traversal of its nodes values.

For each node at position (X, Y), its left and right children respectively will be at positions (X-1, Y-1) and (X+1, Y-1).

Running a vertical line from X = -infinity to X = +infinity, whenever the vertical line touches some nodes, we report the values of the nodes in order from top to bottom (decreasing Y coordinates).

If two nodes have the same position, then the value of the node that is reported first is the value that is smaller.

Return an list of non-empty reports in order of X coordinate.  Every report will have a list of values of nodes.

**Example 1:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2019/01/31/1236_example_1.PNG)
```
Input: [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
Explanation: 
Without loss of generality, we can assume the root node is at position (0, 0):
Then, the node with value 9 occurs at position (-1, -1);
The nodes with values 3 and 15 occur at positions (0, 0) and (0, -2);
The node with value 20 occurs at position (1, -1);
The node with value 7 occurs at position (2, -2).
```
**Example 2:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2019/01/31/tree2.png)
```
Input: [1,2,3,4,5,6,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation: 
The node with value 5 and the node with value 6 have the same position according to the given scheme.
However, in the report "[1,5,6]", the node value of 5 comes first since 5 is smaller than 6.
```
**Note:**:
- The tree will have between 1 and ```1000``` nodes.
- Each node's value will be between ```0``` and ```1000```.

**Labels:**
- 树 Tree
- 困难 Hard

**思路:**
1. 使用DFS递归遍历树, 用hashmap存放值和坐标(x, y)，根节点坐标（0, 0), 左子节点(x - 1, y + 1)， 右子节点（x + 1, y + 1)。对于相同坐标(x,y)的节点存放在数组中。
2. 遍历结束后，首先针对x坐标排序，再针对y坐标排序，对于坐标相等的节点按照结点值的由小到大排序。

**复杂度分析:**
1. 时间复杂度: O(nlogn), n为树的节点数; DFS遍历O(n), 排序O(nlogn)
2. 空间复杂度: O(n)
3. 执行结果：通过
- 执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户
- 内存消耗：13 MB, 在所有 C++ 提交中击败了45.98%的用户

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
class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        map<int, vector<pair<int, int>>> tree;

        // traverse from root node to all nodes and store node info (x, pair<y, val> ) into hashmap
        dfs(root, tree, 0, 0);

        vector<vector<int>> res;

        for (auto it : tree) {
            vector<pair<int, int>> vert = it.second;

            sort(vert.begin(), vert.end(),
            [](const pair<int, int>& a, const pair<int, int>& b) {
                return (a.first < b.first) || (a.first == b.first) && (a.second < b.second); // first compare row:y, if same row then compare value
            });

            vector<int> vals;
            for (auto val : vert)
                vals.push_back(val.second);
            res.push_back(vals);
            vals.clear();
            vert.clear();
        }

        return res;
    }
private:
    void dfs(TreeNode* node, map<int, vector<pair<int,int>>>& tree, int x, int y) {
        if (!node) return;

        tree[x].push_back({y, node->val});

        dfs(node->left, tree, x - 1, y + 1);
        dfs(node->right, tree, x + 1, y + 1);
    }
};
```
