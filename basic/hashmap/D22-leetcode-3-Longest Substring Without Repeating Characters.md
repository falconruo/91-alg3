**题目:**

3. Longest Substring Without Repeating Characters
Given a string ```s```, find the length of the **longest substring** without repeating characters.

**Example 1:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```
**Example 2:**
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```
**Example 3:**
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```
**Example 4:**
```
Input: s = ""
Output: 0
```
**Constraints:**:
- ```0 <= s.length <= 5 * 104```
- ```s``` consists of English letters, digits, symbols and spaces.

**Labels:**
- 哈希表 Hashtable/Hashmap
- 中等 Medium

**思路:**

滑动窗口(双指针)法:
- 用一个指针l记录窗口的左边界, 另一个指针i记录窗口右边界，整个窗口用hashmap记录
- 出现重复字符s[i], 在hashmap里查找重复字符s[i]出现的位置index = dict[s[i]],收缩窗口左边界,将 0 - index之间的字符移出窗口, 在边界从index + 1开始 (l = index + 1)
- 继续滑动右边界i,直至结束

**复杂度分析:**

时间复杂度: O(n), n为字符串长度
空间复杂度: O(n)

执行结果：通过
- 执行用时：28 ms, 在所有 C++ 提交中击败了70.48%的用户
- 内存消耗：8.3 MB, 在所有 C++ 提交中击败了59.17%的用户

**代码(C++):**
```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.length();

        if (n <= 1) return n;

        unordered_map<char, int> dict;

        int maxlen = 0;
        int l = 0;
        for (int i = 0; i < n; ++i) {
            if (dict.count(s[i]) && l <= dict[s[i]])
                l = dict[s[i]] + 1;
            
            dict[s[i]] = i;
            maxlen = max(maxlen, i - l + 1);
        }
    
        return maxlen;
    }
};
```
