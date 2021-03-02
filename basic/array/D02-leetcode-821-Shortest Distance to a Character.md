**题目:**

821. Shortest Distance to a Character

Given a string S and a character C, return an array of integers representing the shortest distance from the character C in the string.

**Example 1:**
```
Input: S = "loveleetcode", C = 'e'
Output: [3, 2, 1, 0, 1, 0, 0, 1, 2, 2, 1, 0]
```

**Note:**
1. `S` string length is in `[1, 10000]`.
2. `C` is a single character, and guaranteed to be in string `S`.
3. All letters in `S` and `C` are lowercase.

**Labels:**
- 数组 Array
- 简单 Easy

**思路:**

查找string S里的每个字符离给定字符C的index的最短距离，对于每个字符有以下三种情况：

。如果等于C则距离=0
。最近的C在其左边
。最近的C在其右边

。从左到右依次遍历字符串S，比较每个字符与字符C的最短距离并存放到返回数组res中
。从右到左依次遍历字符串S，比较每个字符与字符C的最短距离并与之前存放在数组res中的距离取小值

**复杂度分析:**

时间复杂度: O(n), n为数组digits长度
空间复杂度: O(1)

**代码(C++):**

```C++

class Solution {
public:
    vector<int> shortestToChar(string S, char C) {
        int n = S.length();
        vector<int> res(n, INT_MAX);

        int l;
        // scan string S from left to right to get the distance of each character
        for (int i = 0; i < n; ++i) {
            if (S[i] == C) {
                res[i] = 0;
                l = i;
            } else if (l >= 0)
                res[i] = min(res[i], abs(i - l));
        }

        // scan string S from right to lef to get the distance of each character, then choose the shortest one via comparing the distance values
        for (int i = n - 1; i >= 0; --i) {
            if (S[i] == C)
                l = i;
            else
                res[i] = min(res[i], abs(i - l));
        }

        return res;
    }
};
```
