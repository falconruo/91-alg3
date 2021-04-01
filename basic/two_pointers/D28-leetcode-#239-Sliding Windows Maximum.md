**题目:**

239. Sliding Window Maximum

You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

**Example 1:**
```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Example 2:**
```
Input: nums = [1], k = 1
Output: [1]
```

**Example 3:**
```
Input: nums = [1,-1], k = 1
Output: [1,-1]
```

**Example 4:**
```
Input: nums = [9,11], k = 2
Output: [11]
```

**Example 5:**
```
Input: nums = [4,-2], k = 2
Output: [4]
```

**Constraints:**:
- `1 <= nums.length <= 105`
- `-10^4 <= nums[i] <= 10^4`
- `1 <= k <= nums.length`

**Labels:**
- 双指针 Double Pointers (slide-windows)
- 困难 Hard

**思路:**

滑动窗口法, 窗口宽度k, 使用辅助空间-双端队列(double ended queue)或者数组(vector)记录元素的下标
1. 首先如果给定数组小于等于一个元素或者滑动窗口宽度k为1，则直接返回数组nums
2. 从左到右遍历数组,下标i为窗口右边界
    - 窗口(sliding window)的宽度 > k时(i - dq.front() + 1 > k)，整个窗口左边界向右滑动一步，辅助队列的头部元素出队
    - 如果当前队列非空并且元素nums[i]不小于队尾元素时，持续将队尾元素出队
    - 将当前元素nums[i]插入队尾,保证队尾始终应该存放当前窗口的最大值
    - 当前下标超出k时, 队头的下标值对应的数组中的值nums[dq.front()]为当前窗口最大值

**复杂度分析:**

时间复杂度: O(n), n为数组nums的元素数
空间复杂度: O(k), k为窗口宽度

执行结果：通过
- 执行用时：348 ms, 在所有 C++ 提交中击败了89.81%的用户
- 内存消耗：107 MB, 在所有 C++ 提交中击败了62.70%的用户

**代码(C++):**
```C++

class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();

        if (k == 1 || n <= 1) return nums;

        // double ended queue, 单调递减队列
        // use a double-ended queue or an array to store the index i of maximum element of each sliding window
        deque<int> dq;
        // array res stores the maximal element nums[i] of each sliding windows
        vector<int> res;

        for (int i = 0; i < n; ++i) {
            if (!dq.empty() && i - dq.front() + 1 > k) // 超出窗口宽度k，则收缩左边界
                dq.pop_front();
            
            while (!dq.empty() && nums[i] >= nums[dq.back()]) // 保证队尾始终保持当前窗口最大值的下标
                dq.pop_back();

            dq.push_back(i);
            if (i >= k - 1) res.push_back(nums[dq.front()]); // 窗口宽度达到k时, 此时的队头为当前窗口最大值下标
        }
        return res;
    }
};
```