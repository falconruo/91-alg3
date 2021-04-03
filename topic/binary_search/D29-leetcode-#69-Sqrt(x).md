**题目:**

69. Sqrt(x)

Given a non-negative integer x, compute and return the square root of x.

Since the return type is an integer, the decimal digits are truncated, and only the integer part of the result is returned.

**Example 1**:
```
Input: x = 4
Output: 2
```

**Example 2**:
```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
```

**Constraints**:
- `0 <= x <= 2^31 - 1`

**Labels**
- 二分查找 Binary Search
- 简单 Easy

**思路:**

二分查找: 
1. 利用两个指针l, r分别指向0，x
2. 执行循环：
    - 每次取中间值mid, 使其为l与r的和的一半,为了防止溢出注意将mid的类型定义为long
    - 判断mid的平方与值x的关系：
      1) 如果mid的平方小于等于x，而mid + 1的平方大于x, 则表示mid为x的平方根，直接返回mid
      2) 如果仅mid的平方小于等于x，则说明目标值在右半部分，将左指针l向右移动到mid + 1
      3) 如果仅mid的平方大于x，则说明目标值在左半部分，将右指针l向左移动到mid - 1
3. 返回左指针l

**复杂度分析:**

时间复杂度: O(logx)

空间复杂度: O(1)

执行结果：通过
- 执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户
- 内存消耗：5.7 MB, 在所有 C++ 提交中击败了97%的用户

**代码(C++):**
```C++

class Solution {
public:
    int mySqrt(int x) {
        int l = 0, r = x;

        while (l <= r) {
            long mid = l + (r - l)/2;

            if ((mid * mid <= x) && ((mid + 1)*(mid + 1) > x))
                return int(mid);
            else if (mid * mid > x)
                r = mid - 1;
            else
                l = mid + 1;
        }

        return l;
    }
};
```