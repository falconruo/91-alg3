
**题目:**

26. Remove Duplicates from Sorted Array

Given a sorted array nums, remove the duplicates in-place such that each element appears only once and returns the new length.

Do not allocate extra space for another array, you must do this by **modifying the input** array in-place with O(1) extra memory.

**Clarification**:

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means a modification to the input array will be known to the caller as well.

Internally you can think of this:
```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**Example 1:**
```
Input: nums = [1,1,2]
Output: 2, nums = [1,2]
Explanation: Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the returned length.
```

**Example 2:**
```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4]
Explanation: Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively. It doesn't matter what values are set beyond the returned length.
```

**Constraints:**:
- `0 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in ascending order.

**Labels:**
- 双指针 Double Pointers (slide-windows)
- 简单 Easy

**思路:**

双指针法, 一个指针l指向当前不重复项，另一个指针i循环向右:
1. 遍历整个数组比较nums[l]与nums[i]的值
- 相等则i继续右移一位
- 不等则将l右移一位，并将nums[l]的值 = nums[i]
2. 最后返回l + 1

**复杂度分析:**

时间复杂度: O(n), n为数组nums元素个数
空间复杂度: O(1)

执行结果：通过
- 执行用时：20 ms, 在所有 C++ 提交中击败了64.73%的用户
- 内存消耗：13.7 MB, 在所有 C++ 提交中击败了19.63%的用户

**代码(C++):**
```C++

class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) return n;

        // Use double pointers l, i as the left and right index of the new array w/o duplicated elements
        int l = 0;
        for (int i = 0; i < n; ++i)
            if (nums[l] != nums[i])
                nums[++l] = nums[i];

        return l + 1; // the length is the index + 1 since the index starts from 0
    }
};
```
