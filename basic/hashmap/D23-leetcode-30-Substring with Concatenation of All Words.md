**题目:**

30. Substring with Concatenation of All Words

You are given a string s and an array of strings words of the same length. Return all starting indices of substring(s) in s that is a concatenation of each word in words **exactly once, in any order, and without any intervening characters**.

You can return the answer in any order.

**Example 1:**
```
Input: s = "barfoothefoobarman", words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```

**Example 2:**
```
Input: s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
Output: []
```

**Example 3:**
```
Input: s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
Output: [6,9,12]
```

**Constraints:**:
- `1 <= s.length <= 10^4`
- `s` consists of lower-case English letters.
- `1 <= words.length <= 5000`
- `1 <= words[i].length <= 30`
- `words[i]` consists of lower-case English letters.

**Labels:**
- 哈希表 Hashtable/Hashmap
- 困难 Hard

**思路:**

从 s 串入手，遍历 s 串中所有长度为 (words[0].length * words.length) 的子串 Y，查看 Y 是否可以由 words 数组构造生成

仅考虑遍历过程，遍历 s 串的时间复杂度为O(n - m + 1), 其中 n 为 s 字符串长度, m 为 words[0].length * words.length，也就是 words 的字符总数。问题关键在于如何判断 s 的子串 Y 是否可以由 words 数组的构成，由于 words 中单词长度固定，我们可以将 Y 拆分成对应 words[0]长度的一个个子串 parts, 只需要判断 words 和 parts 中的单词是否一一匹配即可，这里用两个哈希表表比对出现次数即可。一旦一个对应不上，意味着此种分割方法不正确，继续尝试下一种即可

使用hashmap存放字符串数组words的每个字符串及次数
- 循环判断字符串，利用另外一个hashmap来存放字符串s中长度为len的子串出现的次数
- 如果次数小于0则表示当前子串不在hashmap中或出现次数大于hashmap中的重复次数，此时退出内层循环
- 如果count == nums表示找到一个符合的类型子串恰好包含words中的所有字符串，将当前starting indice存入res中

**复杂度分析:**

时间复杂度: O(n * m), n为字符串s的长度, m为字符串数组words的字符总长度(words[0].length * words.length)
空间复杂度: O(nums), hashmap空间 = words的字符串个数

执行结果：通过
- 执行用时：904 ms, 在所有 C++ 提交中击败了13.6%的用户
- 内存消耗：285.6 MB, 在所有 C++ 提交中击败了7.38%的用户

**代码(C++):**
```C++
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        int len = words[0].length();
        int num = words.size();

        unordered_map<string, int> wmap;
        vector<int> res;
    
        if (len > s.length())
            return {};

        for (auto w : words)
            wmap[w]++;

        for (int i = 0; i < s.length() - num * len + 1; ++i) { // the start is <= the string length - the total length of words
            int cnt;
            unordered_map<string, int> smap(wmap);
            for (cnt = 0; cnt < num; ++cnt) {
                string sub = s.substr(i + cnt * len, len);
                smap[sub]--;
                if (smap[sub] < 0)
                    break;
            }
            if (cnt == num)
                res.push_back(i);
        }
        return res;
    }
};
```
