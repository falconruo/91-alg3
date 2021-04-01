**题目:**

35. Search Insert Position

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

**Example 1:**
```
Input: nums = [1,3,5,6], target = 5
Output: 2
```

**Example 2:**
```
Input: nums = [1,3,5,6], target = 2
Output: 1
```

**Example 3:**
```
Input: nums = [1,3,5,6], target = 7
Output: 4
```

**Example 4:**
```
Input: nums = [1,3,5,6], target = 0
Output: 0
```

**Example 5:**
```
Input: nums = [1], target = 0
Output: 0
```

**Constraints:**:
- ```1 <= nums.length <= 104```
- ```-10^4 <= nums[i] <= 10^4```
- ```nums``` contains distinct values sorted in ascending order.
```-10^4 <= target <= 10^4```

**Labels:**
- 双指针 Double Pointers (slide-windows)
- 困难 Hard

**思路:**

方法一: 循环遍历整个数组,判断连续相邻的两个元素nums[i - 1], nums[i]与target的大小，确保target落在两者之间即可

方法二: 二分查找
1. 每次取中间位置mid并判断nums[mid]与目标值target的大小 (左闭右开)
- target > nums[mid], 则移动左边界至 mid + 1 ()
- target < nums[mid], 则移动右边界至 mid
- target == nums[mid]，则直接返回mid
2. 默认返回左边界l

**复杂度分析:**

时间复杂度: 方法一: O(n), 方法二: O(logn), n为数组元素个数
空间复杂度: O(1)

执行结果：通过

方法一:
- 执行用时：4 ms, 在所有 C++ 提交中击败了90.17%的用户
- 内存消耗：9.4 MB, 在所有 C++ 提交中击败了32.12%的用户

方法二:
- 执行用时：4 ms, 在所有 C++ 提交中击败了90.17%的用户
- 内存消耗：9.5 MB, 在所有 C++ 提交中击败了21.60%的用户

**代码(C++):**
```C++
//方法一
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n = nums.size();

        if (n == 1)
            return target <= nums[n - 1] ? n - 1: n;

        int i;
        for (i = 1; i < n; ++i) {
            if (nums[i - 1] >= target)
                return (i - 1);
            else if (nums[i] >= target)
                break;
        }

        return i;
    }
};

//方法二
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l = 0;
        int r = nums.size();

        while (l < r) {
            int mid = l + (r - l) / 2;

            if (nums[mid] > target)
                r = mid;
            else if (nums[mid] < target)
                l = mid + 1;
            else
                return mid;
        }
        return l;
    }
};
```