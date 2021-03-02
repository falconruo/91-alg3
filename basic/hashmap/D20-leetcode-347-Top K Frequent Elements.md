**题目:**

347. Top K Frequent Elements

Given a non-empty array of integers, return the k most frequent elements.

**Example 1:**
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**
```
Input: nums = [1], k = 1
Output: [1]
```

**Constraints:**:
- You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
- Your algorithm's time complexity **must be** better than O(n log n), where n is the array's size.
- It's guaranteed that the answer is unique, in other words the set of the top k frequent elements is unique.
- You can return the answer in any order.

**Labels:**
- 哈希表 Hashtable/Hashmap
- 中等 Medium

**思路:**
1. 使用hashmap存放数组的值<key>和frequency<value>
2. 由大到小按照frequency排序:
    - 方法一:传统sort排序
    - 方法二:最小堆(Min Heap),存放最多k个pair
    - 方法三:使用Set特性

**复杂度分析:**

时间复杂度: O(nlogk), n为数组nums的元素个数
空间复杂度: O(m), m为不重复元素个数

执行结果：通过
- 执行用时：12 ms, 在所有 C++ 提交中击败了96.71%的用户
- 内存消耗：9.2 MB, 在所有 C++ 提交中击败了48.11%的用户

**代码(C++):**
```C++
//方法一:
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freq;

        for (auto n : nums) // o(n)
            freq[n]++;
        // only sequence containers can be sorted: such as set, vector
        vector<pair<int, int>> elements(freq.begin(), freq.end());

        sort(elements.begin(), elements.end(),
            [](const pair<int, int> a, const pair<int, int> b) {
                return a.second > b.second;
            }); // O(mlogm)

        vector<int> res;

        for (auto f : elements) { // o(k)
            res.push_back(f.first);
            if (--k == 0) return res;
        }
        return {};
    }
};

//方法二:
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        map<int, int> freq;

        // use a hashmap to store the frequency of each element
        for (auto n : nums) // O(n)
           freq[n]++;

        // use min heap to store the k most frequent elements
        /*
        priority_queue<int, vector<int>, less<int>>s;//less表示按照递减(从大到小)的顺序插入元素
        priority_queue<int, vector<int>, greater<int>>s;//greater表示按照递增（从小到大）的顺序插入元素
        */
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

        for (auto f : freq) { // o(m), m is the unique elements
            if (pq.size() < k)
                pq.push({f.second, f.first}); // stack sort o(logk)
            else if (pq.top().first < f.second) {
                pq.pop();
                pq.push({f.second, f.first});
            }
        }

        vector<int> res;

        while (!pq.empty()) { // O(k)
            res.push_back(pq.top().second);
            pq.pop();
        }

        return res;
    }
};

//方法三
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> counts;
        // save the count of each element into hashtable
        for (auto num : nums)
            counts[num]++;
        
        vector<int> res;
        // sort element from most frequent to less
        set<pair<int, int>> s; // sequencial
        int i = 0;
        for (auto freq : counts) { // O(n)
            ++i;
            s.insert({freq.second, freq.first}); // O(log(n))
            if (i > k && (*s.begin()).first <= freq.second)
                s.erase(*s.begin());
        }
        // store the k most frequent elements into array
        for (auto e : s)
            res.push_back(e.second);

        return res;
    }
};
```