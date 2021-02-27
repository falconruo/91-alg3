**题目:**

297. Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification**: The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Example 1:**

![Aaron Swartz](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)
```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

**Example 2:**
```
Input: root = []
Output: []
```

**Example 3:**
```
Input: root = [1]
Output: [1]
```

**Example 4:**
```
Input: root = [1,2]
Output: [1,2]
```

**Constraints**:
- The number of nodes in the tree is in the range ```[0, 104]```.
- ```-1000 <= Node.val <= 1000```

**Labels:**
- 树 Tree
- 困难 Hard
- 剑指Offer 37

**思路:**

递归法（pre-order): DFS

迭代法（BFS): 使用队列存放节点

**复杂度分析:**

1. 时间复杂度: O(n), n为树的节点数
2. 空间复杂度: DFS O(logn), 编码和递归栈的大小; BFS: O(n), n为树的节点数
3. 前序遍历(DFS)执行结果：通过
    - 执行用时：52 ms, 在所有 C++ 提交中击败了82.47%的用户
    - 内存消耗：30.6 MB, 在所有 C++ 提交中击败了74.68%的用户
4. 迭代(BFS)执行结果：通过
    - 执行用时：60 ms, 在所有 C++ 提交中击败了63.94%的用户
    - 内存消耗：33 MB, 在所有 C++ 提交中击败了44.55%的用户

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
 * Recursive
 */
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        ostringstream out;
        serialize(root, out);
        return out.str();
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        istringstream in(data);
        return deserialize(in);
    }
private:
    // serialize to "1 2 null 3 4 5"
    void serialize(TreeNode* root, ostringstream& out) {
        if (!root) { // nullptr
            out << "null ";
            return;
        }
        
        out << root->val << " ";
        serialize(root->left, out);
        serialize(root->right, out);
    }
    
    // deserialize to tree
    TreeNode* deserialize(istringstream& in) {
        string val;
        
        in >> val;
        
        if (val == "null") return nullptr;
        
        TreeNode* root = new TreeNode(stoi(val));
        root->left = deserialize(in);
        root->right = deserialize(in);
        return root;
    }
};

/**
 * Iterative
 */
class Codec {
public:

    // Encodes a tree to a single string
    string serialize(TreeNode* root) {
        if (!root) return "";

        // BFS + queue
        queue<TreeNode*> tree;
        ostringstream out;

        tree.push(root);

        while (!tree.empty()) {
            TreeNode* cur = tree.front();
            tree.pop();

            if (cur == nullptr)
                out << "null ";
            else {
                out << to_string(cur->val) << " ";
                tree.push(cur->left);
                tree.push(cur->right);
            }
        }

        string s = out.str();

        return s.substr(0, s.length() -1);
    }
  
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data == "") return nullptr;

        istringstream in(data);

        vector<string> vals;
        string val;

        while (!in.eof()) {
            in >> val;
            vals.push_back(val);
        }

        queue<TreeNode*> tree;
        TreeNode* root = new TreeNode(stoi(vals[0]));
        tree.push(root);

        for (int i = 1; i < vals.size(); ++i) {
            TreeNode* node = tree.front();
            tree.pop();
            node->left = nullptr;
            if (vals[i] != "null") {
                node->left = new TreeNode(stoi(vals[i]));
                tree.push(node->left);
            }

            node->right = nullptr;      
            if (vals[++i] != "null") {
                node->right = new TreeNode(stoi(vals[i]));
                tree.push(node->right);
            }
        }

        return root;
  }   
};
```
