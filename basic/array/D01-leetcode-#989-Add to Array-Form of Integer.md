**题目:**

989. Add to Array-Form of Integer

For a non-negative integer `X`, the array-form of X is an array of its digits in left to right order.  For example, if `X = 1231`, then the array form is `[1,2,3,1]`.

Given the array-form `A` of a non-negative integer `X`, return the array-form of the integer `X+K`.
 
**Example 1:**
```
Input: A = [1,2,0,0], K = 34
Output: [1,2,3,4]
Explanation: 1200 + 34 = 1234
```

**Example 2:**
```
Input: A = [2,7,4], K = 181
Output: [4,5,5]
Explanation: 274 + 181 = 455
```

**Example 3:**
```
Input: A = [2,1,5], K = 806
Output: [1,0,2,1]
Explanation: 215 + 806 = 1021
```

**Example 4:**
```
Input: A = [9,9,9,9,9,9,9,9,9,9], K = 1
Output: [1,0,0,0,0,0,0,0,0,0,0]
Explanation: 9999999999 + 1 = 10000000000
```

**Note:**
1. `1 <= A.length <= 10000`
2. `0 <= A[i] <= 9`
3. `0 <= K <= 10000`
4. If `A.length > 1`, then `A[0] != 0`

**Labels:**
- 数组 Array
- 简单 Easy

**思路:**

方法一、
1. 循环，退出条件数组A的每一位都遍历完或者数字K为0
    - 从数组A的最高位依次与给定整数K相加，然后将结果赋给K
    - 将K取余数 (K % 10)放入返回数组res
    - 将K取商(k /10)并赋给K
2. 翻转返回数组res
3. 返回数组res

方法二、
1. 利用给定的数组A来存放结果
2. 首先将数组A全部翻转，翻转后A[0]为数组A代表的数字的最低位，循环直至遍历数组A:
    - 将A[i]与K相加, 并赋给K
    - 取余的结果存入A[i]
    - 取商赋给K
3. 循环直至K为0
    - K取余的结果存入数组A
    - 取商赋给K
4. 再次翻转数组A并返回

**复杂度分析:**

时间复杂度: O(max(n, lgK)), n为数组元素A的元素个数(遍历数组一遍), lgK为整数K的位数

空间复杂度: O(1)

执行结果：通过
- 执行用时：24 ms, 在所有 C++ 提交中击败了93.00%的用户
- 内存消耗：25.4 MB, 在所有 C++ 提交中击败了83.34%的用户

**代码(C++):**
```C++

方法一、
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& A, int K) {
        vector<int> res;

        int i = A.size() - 1;
        while (i >= 0  || K) {
            if (i < A.size()) {
                K += A[i];
                i--;
            } 
            res.push_back(K % 10);
            K /= 10;
        }

        reverse(res.begin(), res.end());
        return res;
    }
};

方法二、
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& A, int K) {
        vector<int> res;

        reverse(A.begin(), A.end());
        int i = 0;

        while (i < A.size()) {
            K += A[i];
            A[i] = K % 10;
            K /= 10;
            i++;
        }

        while (K) {
            A.push_back(K % 10);
            K /= 10; 
        }

        reverse(A.begin(), A.end());
        return A;
    }
};
```
