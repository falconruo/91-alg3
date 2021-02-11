**题目:**

394. Decode String

Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

**Example 1:**
```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

**Example 2:**
```
Input: s = "3[a2[c]]"
Output: "accaccacc"
```

**Example 3:**
```
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```

**Example 4:**
```
Input: s = "abc3[cd]xyz"
Output: "abccdcdcdxyz"
```

**Labels:**
- 栈 Stack
- 中等 Medium

**思路:**
方法一、
迭代， 循环遍历整个给定的字符串s,使用一个栈stack<string> st来存放字符子串
1. 对于数字字符，循环直至碰到不是数字的字符，将整个数字字符子串作为repeated time入栈st中，注意：需要将字符串s的index回退一位
2. 对于左方括号(open bracket '['), 作为单独的字符串存入栈st中
3. 对于字母字符('a' - 'z', 'A' - 'Z'), 循环直至碰到不是字母的字符，将整个字母字符子串作为encoded 字符串存入栈st中, 注意：需要将index回退一位
4. 对于右方括号(close bracket ']')
  - 循环从栈st中读出栈顶的字母字符串并将后面读出的字符串插入到前面，每次出栈栈顶元素，直至栈顶字符串不是字母字符串，此时整个encoded string即为需要repeat的字符串
  - 如果栈顶字符串是'['，直接pop出栈
  - 如果栈顶字符串是数字字符串，此数字即为之前的endcoded string字母字符串需要repeated的次数, 根据repeated times生成新的decoded字符串并存入栈st

方法二、
递归，使用DFS深度优先遍历方法

**复杂度分析:**
1. 时间复杂度: O(m), m为给定字符串s长度
2. 空间复杂度: O(n), n为decoded string长度
3. 执行结果：通过
    迭代法
    - 执行用时：0 ms, 在所有 C++ 提交中击败了100%的用户
    - 内存消耗：6.4 MB, 在所有 C++ 提交中击败了86.92%的用户

    递归法
    - 执行用时：4 ms, 在所有 C++ 提交中击败了94%的用户
    - 内存消耗：6.3 MB, 在所有 C++ 提交中击败了93.82%的用户

**代码(C++):**
```C++
语言： cpp

方法一、
class Solution {
public:
    string decodeString(string s) {
        stack<string> st;
        string res = "";

        for (int i = 0; i < s.length(); ++i) {
            string encoded_s = "";

            if (isdigit(s[i])) {
                while (isdigit(s[i])) {
                    encoded_s.push_back(s[i]);
                    ++i;
                }
                st.push(encoded_s);
                --i; // move back one step for the non-number character
            } else if (s[i] == '[') {
                encoded_s = s[i];
                st.push(encoded_s);
            } else if (isalpha(s[i])) {
                while (isalpha(s[i])) {
                    encoded_s.push_back(s[i]);
                    ++i;
                }
                st.push(encoded_s);
                --i; // move back one step for the non-number character
            } else if (s[i] == ']') {
                while (st.top() != "[") {
                    encoded_s.insert(0, st.top());
                    st.pop();
                }

                st.pop();
                int repeat = stoi(st.top());
                st.pop();

                string decode_s = "";

                while (repeat--)
                    decode_s += encoded_s;
                st.push(decode_s);
            }
        }

        while (!st.empty()) {
            res.insert(0, st.top());
            st.pop();
        }

        return res;
    }
};

方法二、
class Solution {
public:
    string decodeString(string s) {
        int i = 0;
        return dfs(s, i);
    }
private:
    string dfs(string& s, int& i) {
        string res = "";
        int repeat = 0;

        for (; i < s.length(); i++) {
            if (isdigit(s[i])) {
                repeat = repeat * 10 + s[i] - '0';
            } else if (s[i] == '[') {
                string encode_s = dfs(s, ++i);
                
                while (repeat) {
                    res += encode_s;
                    repeat--;
                }
            } else if (s[i] == ']') {
                return res;
            } else
                res += s[i];
        }

        return res;
    }
};
```
