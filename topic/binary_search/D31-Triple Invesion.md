**题目:**

493. Reverse Pairs
Given an integer array nums, return the number of reverse pairs in the array.

A reverse pair is a pair (i, j) where 0 <= i < j < nums.length and nums[i] > 2 * nums[j].

**Example 1:**
```
Input: nums = [1,3,2,3,1]
Output: 2
```
**Example 2:**
```
Input: nums = [2,4,3,5,1]
Output: 3
```
**Constraints**:
- `1 <= nums.length <= 5 * 104`
- `2^31 <= nums[i] <= 2^31 - 1`

Similar:
Triple Inversion

Given a list of integers `nums`, return the number of pairs `i < j` such that `nums[i] > nums[j] * 3`.

**Example 1**:
```
Input: nums = [7, 1, 2]
Output: 2
Explanation:
We have the pairs (7, 1) and (7, 2)
```
**Constraints**:
- `n <= 100,000` where `n` is the length of `nums`

**Labels**
- 二分查找 Binary Search
- 困难 (Hard)

**思路:**
数组中的逆序对(reversePairs)：数组中两个数字i, j，满足条件则组成逆序对
- 前面一个数字(下标i)大于后面(下班)的数字: nums[i] > nums[j]
- 下标i < 下标j

题目是查找满足条件的逆序对, 使得nums[i] > nums[j] * 3

一、暴力法 O(n^2)
使用两遍循环的方法，依次计算每一个nums[i]的小于三倍的元素个数，将所有满足的个数求和

二、二分法 worst case: O(n^2)
使用两次二分法, 维护一个有序序列(单调栈或数组)来存放， 第一次用于查找小于当前数的3倍的元素个数(nums[j] < 3 * nums[i])，另一次查找的当前数在有序序列里的插入位置，利用二分法的灵魂-对解空间的折半: 
1. 利用两个指针l, r分别指向1，n - 1
2. 执行循环：
    - 每次取中间值mid, 使其为l与n的和的一半(mid = l + (r - l)/2)
    - 如果sortedNums[mid] <= target，则移动左边界l = mid + 1， 否则移动右边界 r = mid - 1
      1) 
      2) 如果输入值mid - 1返回值true,则mid不是first bad version，将右指针r向左移动到mid-1
      3) 如果输入值mid返回值flase, 则first bad version的下标大于mid, 将左指针l向右移动到mid + 1
3. 返回左指针l

三、树状数组(BIT, Binary Indexed Tree/Fenwick Tree)
BIT provides a way to represent an array of numbers in an array(can be visualized as tree), allowing prefix/suffix sums to be calculated efficiently (O(logn)). BIT allows to update an element in O(logn) time.

BIT is very useful to accumulate information from front/back and hence, we can use it in the same way we used BST to get the count of elements that are greater than or equal to 3 * nums[i] + 1 in the existing tree and then adding the current element into the tree.

The main idea is to count the number of elements greater than 3 * nums[i] in range [0, i) as we iterate from 0 to size - 1.

四、归并排序
如果i, j分别属于两个有序区间，并且 nums[i] > nums[j] * 3, 则大于i的元素也都满足条件, 利用有限特点来减少重复的比较操作。

将两个有序的序列(数组等)合并成一个有序序列成为归并。归并排序包含了两个过程:
1. 从上往下的分解: 把当前区间一分为二，直至分解为若干个长度为1的子序列
2. 从下往上的合并: 两个有序的子区间两两向上合并

归并排序的一般步骤:
1. 分解: 把当前区间一分为二，分解点 mid = l + (r - l) / 2
2. 求解: 分别递归左右两个子区间[l, mid]和[mid + 1, r]进行归并排序。递归的终结条件是子区间的长度为1
3. 合并: 把两个有序子区间合并需要占用一个临时空间，依次移动两个子区间的指针，比较元素值的大小，将较小的值存入临时空间的开头。将两个有序区间合并成一个临时有序区间[l, r]，并将结果拷贝到原始序列的区间[l, r]，使得原始序列[l, r]变为有序

**复杂度分析:**

时间复杂度: 1. O(nlogn)

空间复杂度: O(n)

执行结果：通过
- 执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户
- 内存消耗：5.7 MB, 在所有 C++ 提交中击败了98.5%的用户

**代码(C++):**
```C++

class Solution {
public:
    int reversePairs(vector<int>& nums) {
        int size = nums.size();
        int l = 0;
        int r = size-1;
        int res = 0;
        vector<int> tmp(size);
        mergesort(nums, tmp, l, r, res);
        return res;
    }

    void mergesort(vector<int>& nums, vector<int>& tmp, int l,int r,int &res)
    {
        if( l >= r) return; //基线条件，只剩一个元素就是有序的了
        int mid = l + (r - l)/2; //分隔点
        mergesort(nums, tmp, l, mid, res);//对左区间排序
        mergesort(nums, tmp, mid + 1, r,res);//对右区间排序
        
        int i = l;
        int j = mid + 1;
        while( i <= mid && j <= r )//左右区间都是有序的了，进行统计操作
        {
            if( (long long )nums[i] > (long long )2 * nums[j])// *2 会超 int 范围
            {
                res += (mid - i + 1);//反转对个数
                j++;
            }
            else
                i++;
        }

        i = l;
        j = mid + 1;
        int t = 0;
        while( i <= mid && j <= right )//进行归并操作
            if( nums[i] > nums[j])
                temp[t++] = nums[j++];
            else
                temp[t++] = nums[i++];
        while( i <= mid )//将左边剩余元素填充进temp中
            temp[t++] = nums[i++];

        while( j <= right )//将右序列剩余元素填充进temp中
            temp[t++] = nums[j++];
        t = 0;
        while( left <= right )//将temp中的元素全部拷贝到原数组[left,right]中
            nums[left++] = temp[t++];
    }
};
```