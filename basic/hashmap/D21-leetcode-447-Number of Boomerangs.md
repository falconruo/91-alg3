**题目:**

447. Number of Boomerangs

You are given ```n``` points in the plane that are all distinct, where ```points[i] = [xi, yi]```. A **boomerang** is a tuple of points ```(i, j, k)``` such that the distance between ```i``` and ```j``` equals the distance between ```i``` and ```k``` **(the order of the tuple matters)**.

Return the number of boomerangs.

**Example 1:**
```
Input: points = [[0,0],[1,0],[2,0]]
Output: 2
Explanation: The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]].
```

**Example 2:**
```
Input: points = [[1,1],[2,2],[3,3]]
Output: 2
```

**Example 3:**
```
Input: points = [[1,1]]
Output: 0
```

**Constraints:**:
- ```n == points.length```
- ```1 <= n <= 500```
- ```points[i].length == 2```
- ```-104 <= xi, yi <= 104```
- All the points are unique.

**Labels:**
- 哈希表 Hashtable/Hashmap
- 中等 Medium

**思路:**
1. 使用双重循环，依次计算每一个点与其他n - 1个点之间的距离, 因为距离(distance)相同的任意三个点的tuple的顺序不同,形成的boomerang也不相同,所以对同一个distance的k个点, 从其中取出任意2个(j, k)与当前点(i)组成tuple, 排列 = k * (k - 1)
2. 在里层循环里, 计算当前点i与其n - 1个点的距离, 作为hashmap的key; 相同距离的不同点的数目作为hashmap的value
3. 在外层循环,依次计算当前点的不同距离的tuple顺序的排列数

**复杂度分析:**

时间复杂度: O(n^2), n为points个数
空间复杂度: O(n)

执行结果：通过
- 执行用时：844 ms, 在所有 C++ 提交中击败了61.77%的用户
- 内存消耗：121.8 MB, 在所有 C++ 提交中击败了67.46%的用户

**代码(C++):**
```C++
class Solution {
public:
    int numberOfBoomerangs(vector<vector<int>>& points) {
        int n = points.size();
        
        int res = 0;
        for (int i = 0; i < n; ++i) {
            unordered_map<int, int> dist; // key: distance, value: number of points with same distance
            for (int j = 0; j < n; ++j) {
                if (i != j) {
                    int x = points[i][0] - points[j][0];
                    int y = points[i][1] - points[j][1];

                    int d2 = x * x + y * y;

                    dist[d2]++;
                }
            }

            for (auto it : dist)
                res += it.second * (it.second - 1); //for all the groups of points, number of ways to select 2 from n together with current point to make a boomerang = n!/(n-2)! = n * (n-1)
        }

        return res;
    }
};
```
