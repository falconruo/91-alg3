**题目:**

493. Minimum Light Radius
You are given a list of integers nums representing coordinates of houses on a 1-dimensional line. You have `3` street lights that you can put anywhere on the coordinate line and a light at coordinate `x` lights up houses in `[x - r, x + r]`, inclusive. Return the `smallest r` required such that we can place the 3 lights and all the houses are lit up.

**Example 1:**
```
Input: nums = [3, 4, 5, 6]
Output: 0.5
Explanation:
If we place the lamps on 3.5, 4.5 and 5.5 then with r = 0.5 we can light up all 4 houses.
```

**Constraints**:
- `n <= 100,000` where `n` is the length of `nums`

**Labels**
- 二分查找 Binary Search（二分查找的灵魂是有序序列）
- 困难 (Hard)

**思路:**
1. 首先，思考了一下给的nums数组是否有序，事实证明是无序的，那先排个序
2. 因为半径可能有小数，比较麻烦，可以改成求最小直径，因为房子的坐标都是整数，那直径是房子坐标的差也一定是整数
3. 因为有三盏灯，直径的最大值是最右侧房子和最左侧房子坐标之差的1/3，因此右指针初始化为right = (nums[-1] - nums[0]) // 3
4. 如果当前直径mid可以覆盖所有的房子(allCovered == True)，当前mid可能偏大，也可能就是最小直径，令 right= mid - 1(如果确实偏大就丢弃了，如果确实是最小直径那循环会停止，left就是最小直径，返回left / 2)
5. 如果当前直径mid不可以覆盖所有房子(allCovered == False)，当前mid偏小，增大mid，left = mid + 1
6. 找到最小直径left后返回left的一半即为最小半径

判断是否可以覆盖所有房子的方法：
1. 三盏灯中的两盏肯定是放在两边的，就看中间那盏灯能不能覆盖剩下的房子，也就是看除去左右两盏灯覆盖的范围后的中间区域的房子的左右边界之差有没有比最小直径更小。
2. 通过二分法找中间区域房子的左边界leftBorder和右边界rightBorder
3. 如果右边界超出索引范围返回True（只有nums中地址不重复的房子数量<=3的时候会超出边界，这时候一定是返回True的）
4. 如果左右边界之差小于等于一盏灯可以覆盖的范围，返回True
5. 如果左右边界之差大于一盏灯可以覆盖的范围，返回False

方法二、
先给数组直接排序，毕竟题目中也没有说是有序的，二分法也需要在有序中查找。
1. 先给这个 r 划定一个范围
    - 由于坐标点的距离是整数，因此相互之间的距离也是整数，这里先不考虑半径，考虑直径 d ，那么直径 d 也应该是整数。
    - 直径的最小范围就是 0，最大范围不应该超过数组两边最大距离 L 的 1/3，超过 1/3 就能直接覆盖整条路了。
2. 然后就是在 [0, L] 这样一个范围中查找需要的一个值，满足能覆盖所有 houses，且这个值是最小的，这时候就可以使用二分法了。
3. 判断 d 是否满足能覆盖所有 houses，一共就三个灯，那就一个一个来找位置。
    - 第一盏灯能覆盖的范围肯定是 [nums[0], nums[0] + d]， 因此只要找 nums[0] + d 在排序后的 nums 中找对应的位置，这样就能找到左边第一个没有被第一盏灯覆盖到的 house leftIndex。这里使用二分法即可。
    - 第二盏灯就可以从上面一步找到 的leftIndex 开始计算范围，即第二盏灯能覆盖的范围应该是 [nums[leftIndex], nums[leftIndex] + d]，也就是在 nums 中找到 nums[leftIndex] + d 所在的位置。再次使用二分法。
    - 第三盏灯比较容易了，就看剩下的 house 能不能都被覆盖到。

**关键点**
- 先需要划定 r 的范围，然后使用二分法求解
- 确定灯的位置是否满足条件，即覆盖所有 houses
- 注意还会存在同一个位置的 house

**复杂度分析:**

时间复杂度: 1. O(nlogn)， 排序最费时

空间复杂度: O(1)

执行结果：通过
- 执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户
- 内存消耗：5.7 MB, 在所有 C++ 提交中击败了98.5%的用户

**代码(C++):**
```C++

实现一、
class Solution {
public:
    double solve(vector<int>& nums) {
        int n = nums.size();
        int l = 0, r = nums[n - 1] - nums[0]; // at first let the window cover the whole houses
        // ordered sequence
        if (n <= 3) return 0;
        // the distance of every two houses is not the same
        sort(nums.begin(), nums.end());

        // light radius is the distance of a light to the most left house
        // the diameter of a light is the window [x - r, x + r] of litting up x houses
        while (l <= r) {
            int mid = l + (r - l)/2; // set radius as a half
            if (allLitUp(nums, mid, n)) // the radius is bigger than expected minimum value
                r = mid - 1; // shrink the right border
            else
                l = mid + 1; // shrink the left border
        }

        return (double)r/2;
    }

private:
    bool allLitUp(vector<int>& nums, int d, int n) {
        int cur = nums[0];
        int h = 0; // count of houses

        for (int i = 0; i < 3; ++i) {
            while (h < n && nums[h] <= cur + d) // find the most right house that light i can lit up
                h++;
            if (h == n) break; // all houses are lit up
            cur = nums[h]; // move the left border of window (lit up), ignore the distance of light x and house h, the new window of next light starts from nums[h]
        }
        return h == n;
    }
};

实现二、
class Solution {
public:
    double solve(vector<int>& nums) {
        int n = nums.size();
        // ordered sequence
        if (n <= 3) return 0;

        // the distance of every two houses may not be the same
        sort(nums.begin(), nums.end());

        int l = 0, r = (nums[n - 1] - nums[0])/3; // use 3 lights to cover all houses, so the diameter (r) is 1/3 of the distance of most left house and most right house

        // light radius is the distance of a light to the most left house
        // the diameter of a light is the window [x - r, x + r] of litting up x houses
        while (l <= r) {
            int mid = l + (r - l)/2; // set radius as a half
            if (allLitUp(nums, mid, n)) // the radius is bigger than expected minimum value
                r = mid - 1; // shrink the right border
            else
                l = mid + 1; // shrink the left border
        }

        return (double)l/2;
    }

private:
     /* 判断是否能够照亮所有的 houses */
    bool allLitUp(vector<int>& nums, int d, int n) {
        int cur = nums[0]; // cur + d is the right border of a light

        /* 使用二分法查找 第一盏灯无法照亮的最左第一盏灯 leftIndex */
        int lIndex = 0;
        int rIndex = n;
        int m;
        // first light
        while (lIndex <= rIndex) {
            m = lIndex + (rIndex - lIndex)/2;
            if (nums[m] <= cur + d)
                lIndex = m + 1
            else
                rIndex = m - 1;
        }
        /* 如果出现第一盏灯就能照亮所有的灯，则满足条件返回 */
        if (lIndex >= n) return true;

        /* 再次使用二分法查找 第二盏灯无法照亮的最左第一盏灯 leftIndex */
        // second light
        cur = nums[lIndex] + d;
        rIndex = n;
        while (lIndex <= rIndex) {
            m = lIndex + (rIndex - lIndex)/2;
            if (nums[m] <= cur + d)
                lIndex = m + 1
            else
                rIndex = m - 1;
        }
        /* 如果出现前两盏灯就能照亮所有的灯，则满足条件返回 */
        if (lIndex >= n) return true;

        /* 判断最后一盏灯是否能够照亮剩下的所有灯 */
        // check the thrid light
        return (nums[lIndex] + d >= nums[n - 1])；
    }
};
```