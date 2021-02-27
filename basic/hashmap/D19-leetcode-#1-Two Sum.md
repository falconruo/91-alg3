**题目:**

1. Two Sum

Given an array of integers ```nums``` and an integer ```target```, return indices of the two numbers such that they add up to ```target```.

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

You can return the answer in any order.

**Example 1:**
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```
**Example 2:**
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```
**Example 3:**
```
Input: nums = [3,3], target = 6
Output: [0,1]
```
**Constraints:**:
- ```2 <= nums.length <= 10^3```
- ```-10^9 <= nums[i] <= 10^9```
- ```-10^9 <= target <= 10^9```
- **Only one valid answer exists**.

**Labels:**
- 哈希表 Hashtable/Hashmap
- 简单 Easy

**思路:**

使用hashmap存放数组的值和indices, 循环判断数组的每一个元素,如果target - nums[i]在hashmap中则nums[i]必在, 返回两者的indices

**复杂度分析:**

时间复杂度: O(n), n为数组nums的元素个数
空间复杂度: O(n), hashmap空间

执行结果：通过
- 执行用时：4 ms, 在所有 C++ 提交中击败了96.1%的用户
- 内存消耗：8.7 MB, 在所有 C++ 提交中击败了75.00%的用户

**代码(C++):**
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> used;
        int n = nums.size();

        for (int i = 0; i < n; ++i) {
            if (used.count(target - nums[i]))
                return {used[target - nums[i]], i};
            used[nums[i]] = i;
        }

        return {};
    }
};
```
