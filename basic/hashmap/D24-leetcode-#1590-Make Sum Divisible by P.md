**题目:**

1590. Make Sum Divisible by P

Given an array of positive integers nums, remove the **smallest** subarray (possibly **empty**) such that the **sum** of the remaining elements is divisible by p. It is **not** allowed to remove the whole array.

Return the length of the smallest subarray that you need to remove, or -1 if it's impossible.

A **subarray** is defined as a contiguous block of elements in the array.

**Example 1:**
```
Input: nums = [3,1,4,2], p = 6
Output: 1
Explanation: The sum of the elements in nums is 10, which is not divisible by 6. We can remove the subarray [4], and the sum of the remaining elements is 6, which is divisible by 6.
```

**Example 2:**
```
Input: nums = [6,3,5,2], p = 9
Output: 2
Explanation: We cannot remove a single element to get a sum divisible by 9. The best way is to remove the subarray [5,2], leaving us with [6,3] with sum 9.
```

**Example 3:**
```
Input: nums = [1,2,3], p = 3
Output: 0
Explanation: Here the sum is 6. which is already divisible by 3. Thus we do not need to remove anything.
```

**Example 4:**
```
Input: nums = [1,2,3], p = 7
Output: -1
Explanation: There is no way to remove a subarray in order to get a sum divisible by 7.
```

**Example 5:**
```
Input: nums = [1000000000,1000000000,1000000000], p = 3
Output: 0
```

**Constraints:**:
- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^9`
- `1 <= p <= 10^9`

**Labels:**
- 哈希表 Hashtable/Hashmap
- 中等 Medium

**思路:**

使用前缀和 + 同余定理
- 由前缀和我们知道子数组 A[i:j] 的和就是 pres[j] - pres[i-1]，其中 pres 为 A 的前缀和p
- 由同余定理我们知道两个模 k 余数相同的数字相减，得到的数组一定可以整除p

使用hashmap存放前缀和模 p 的余数 (<key>: 前缀和模 p 的余数; <value>: 前缀和A[i]的下标，目标是找到一个最小的子数组，其模p的余数 = 整个数组和的余数(mod)

**复杂度分析:**

时间复杂度: O(n), n为数组nums的大小
空间复杂度: O(min(n, p)), p为除数

执行结果：通过
- 执行用时：200 ms, 在所有 C++ 提交中击败了91.6%的用户
- 内存消耗：64.9 MB, 在所有 C++ 提交中击败了91%的用户

**代码(C++):**
```C++
class Solution {
public:
    int minSubarray(vector<int>& nums, int p) {
        // To avoid integer overflow (32-bit) using long long
        long long mod = 0, sum = 0;
        for(int i = 0; i < nums.size(); i++)
        {
            mod = (mod + nums[i])%p;
            sum += nums[i];
        }
        if(mod == 0)//余数为0，不需要操作
            return 0;
        if(sum < p)//和小于p, 做不到
            return -1;
        int s = 0, minlen = INT_MAX;
        // s 是求模后的和
        unordered_map<int, int> m;//记录和求模后的数，及其位置
        m[0] = -1;//初始条件，0 在 -1 位置
        for(int i = 0; i < nums.size(); i++)
        {
            s = (s + nums[i])%p;
            int target = (s-mod+p)%p;
            //检查跟 s 相差 需要的余数mod 的余数存在吗
            if(m.find(target) != m.end())
            {
                minlen = min(minlen, i-m[target]);
            }
            m[s] = i;//更新位置
        }
        return minlen >= nums.size() ? -1 : minlen;
    }
};
```