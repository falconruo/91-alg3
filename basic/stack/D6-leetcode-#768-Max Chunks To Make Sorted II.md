**题目:**

768. Max Chunks To Make Sorted II

This question is the same as *Max Chunks to Make Sorted* except the integers of the given array are not necessarily distinct, the input array could be up to length `2000`, and the elements could be up to `10**8`.

Given an array `arr` of integers (not necessarily distinct), we split the array into some number of "chunks" (partitions), and individually sort each chunk.  After concatenating them, the result equals the sorted array.

What is the most number of chunks we could have made?

**Example 1:**
```
Input: arr = [5,4,3,2,1]
Output: 1
Explanation:
Splitting into two or more chunks will not return the required result.
For example, splitting into [5, 4], [3, 2, 1] will result in [4, 5, 1, 2, 3], which isn't sorted.
```

**Example 2:**
```
Input: arr = [2,1,3,4,4]
Output: 4
Explanation:
We can split into two chunks, such as [2, 1], [3, 4, 4].
However, splitting into [2, 1], [3], [4], [4] is the highest number of chunks possible.
```

**Note:**
- `arr` will have length in range `[1, 2000]`.
- `arr[i]` will be an integer in range `[0, 10**8]`.

**Labels:**
- 困难 Hard
- 排序 Sort
- 栈 Stack

**思路:**

方法一、
滑动窗口法
1. 使用一个辅助数组sorted，将数组arr排序后的放入数组sorted
2. 对于一个有效的分块(chunk)，里面的数字之和是相等的，只是数字的顺序不同
3. 使用滑动窗口同时从左向右扫描原数组arr和排序数组sorted,每次当两个窗口里的数字之和相等时，找到一个有效的块(chunk)

方法二、
使用单调栈

因为把数组分成数个块，分别排序每个块后，组合所有的块就跟整个数组排序的结果一样，这就意味着后面块中的最小值一定大于前面块的最大值,这样才能保证分块有效，否则无效,需要合并之前的分块儿；极限情况是每个元素都是独立的块

使用一个辅助递增数组或栈来存放分块的最大值，每个最大值表示一个有效分块，循环给定的数组的每个元素：
1. 如果当前元素大于等于数组尾或栈顶的元素，则表示分块有效，则将当前数组元素作为新的分块的最大值存入辅助数组中;
2. 如果当前元素小数组尾或栈顶的元素则表示当前的分块无效，需要将当前数组元素归于之前的分块中而不是新的分块，此时需要合并之前的一个或数个分块，合并的方法是首先记录辅助数组尾部元素作为当前分块的最大值，依次将当前数组元素与辅助数组中的尾部元素比较，如果当前数组元素更小则将辅助数组尾部向前移动直至辅助数组为空或者辅助数组尾部元素值大于等于给定数组当前元素，此时表示合并分块完成，然后将当前分块的最大值再次存入辅助数组中
3. 辅助递增数组的长度即为有效分块的个数

**复杂度分析:**
方法一、
1. 时间复杂度: O(nlogn), n为给定数组的元素个数
2. 空间复杂度: O(n), n为给定数组的元素个数(辅助数组的长度)
3. 执行结果：通过
    - 执行用时：4 ms, 在所有 C++ 提交中击败了85%的用户
    - 内存消耗：11.8 MB, 在所有 C++ 提交中击败了95%的用户

方法二、
1. 时间复杂度: O(n), n为给定数组的元素个数
2. 空间复杂度: O(n), n为给定数组的元素个数
3. 执行结果：通过
    - 执行用时：12 ms, 在所有 C++ 提交中击败了82.72%的用户
    - 内存消耗：12.3 MB, 在所有 C++ 提交中击败了89.04%的用户

**代码(C++):**
```C++

方法一、
class Solution {
public:
    int maxChunksToSorted(vector<int>& arr) {
        vector<int> sorted = arr;

        sort(sorted.begin(), sorted.end()); // O(nlogn)

        int chunks = 0;

        long sum = 0, sorted_sum = 0;
        for (int i = 0; i < arr.size(); ++i) { // O(n)
            sum += arr[i];
            sorted_sum += sorted[i];

            if (sum == sorted_sum)
                chunks++;
        }

        return chunks;
    }
};

方法二、 使用辅助栈(数组)
class Solution {
public:
    int maxChunksToSorted(vector<int>& arr) {
        stack<int> s;

        for (int i = 0;i < arr.size();i++) {
            if (s.empty() || s.top() <= arr[i])
                s.push(arr[i]);
            else {
                int max_val = s.top();

                while (!s.empty() && s.top() > arr[i])
                    s.pop();
                s.push(max_val);
            }
        }

        return s.size();
    }
};
```
