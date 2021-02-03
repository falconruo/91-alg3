**题目:**

1381. Design a Stack With Increment Operation

Design a stack which supports the following operations.
Implement the ```CustomStack``` class:
- ```CustomStack(int maxSize)``` Initializes the object with maxSize which is the maximum number of elements in the stack or do nothing if the stack reached the maxSize.
- ```void push(int x)``` Adds x to the top of the stack if the stack hasn't reached the maxSize.
- ```int pop()``` Pops and returns the top of stack or -1 if the stack is empty.
- ```void inc(int k, int val)``` Increments the bottom k elements of the stack by val. If there are less than k elements in the stack, just increment all the elements in the stack.

**Example 1:**
```
Input
["CustomStack","push","push","pop","push","push","push","increment","increment","pop","pop","pop","pop"]
[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]
Output
[null,null,null,2,null,null,null,null,null,103,202,201,-1]
Explanation
CustomStack customStack = new CustomStack(3); // Stack is Empty []
customStack.push(1);                          // stack becomes [1]
customStack.push(2);                          // stack becomes [1, 2]
customStack.pop();                            // return 2 --> Return top of the stack 2, stack becomes [1]
customStack.push(2);                          // stack becomes [1, 2]
customStack.push(3);                          // stack becomes [1, 2, 3]
customStack.push(4);                          // stack still [1, 2, 3], Don't add another elements as size is 4
customStack.increment(5, 100);                // stack becomes [101, 102, 103]
customStack.increment(2, 100);                // stack becomes [201, 202, 103]
customStack.pop();                            // return 103 --> Return top of the stack 103, stack becomes [201, 202]
customStack.pop();                            // return 202 --> Return top of the stack 102, stack becomes [201]
customStack.pop();                            // return 201 --> Return top of the stack 101, stack becomes []
customStack.pop();                            // return -1 --> Stack is empty return -1.
```

**Constraints:**
- ```1 <= maxSize <= 1000```
- ```1 <= x <= 1000```
- ```1 <= k <= 1000```
- ```0 <= val <= 100```
- At most ```1000``` calls will be made to each method of increment, push and pop each separately.

**Labels:**
- 数组Array/堆栈Stack
- 中等

**思路:**
1. 使用一个数组vector来模拟栈, 使用一个变量size表示栈的容量maxSize
2. 对于每次入栈函数(push),首先判断当前数组长度(是否小于最大长度size
    - 符合则存入数组
    - 不符合则忽略
3. 对于每次出栈函数(pop)，首先判断当前数组长度是否大于0
    - 符合则将返回数组最后一位
    - 不符合则返回-1
4. 对于将前k个元素的值加给定的val函数(inc), 首先取当前数组长度与k之间取小值len, 循环将前len个元素的值加上val

**复杂度分析:**
1. 时间复杂度: O(k), k为给定长度, 其中push/pop: O(1)
2. 空间复杂度: O(n), n为maxSize
3. 执行结果：通过
    - 执行用时：56 ms, 在所有 C++ 提交中击败了97.55%的用户
    - 内存消耗：18.2 MB, 在所有 C++ 提交中击败了23.16%的用户

**代码(C++):**
```C++
语言： cpp

class CustomStack {
public:
    CustomStack(int maxSize) {
        size = maxSize;
    }
    
    void push(int x) {
        if (st.size() < size)
            st.push_back(x);
    }
    
    int pop() {
        if (st.size()) {
            int res = st.back();
            st.pop_back();
            return res;
        }

        return -1;
    }
    
    void increment(int k, int val) {
        int len = (k <= st.size()) ? k : st.size();

        for (int i = 0; i < len; ++i)
            st[i] += val;
    }
private:
    vector<int> st;
    int size;
};

/**
 * Your CustomStack object will be instantiated and called as such:
 * CustomStack* obj = new CustomStack(maxSize);
 * obj->push(x);
 * int param_2 = obj->pop();
 * obj->increment(k,val);
 */
```
