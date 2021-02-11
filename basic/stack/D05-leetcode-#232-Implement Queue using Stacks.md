
**题目:**

232. Implement Queue using Stacks

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:
- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns true if the queue is empty, false otherwise.

**Notes:**
- You must use only standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

**Follow-up:** Can you implement the queue such that each operation is `amortized O(1)` time complexity? In other words, performing n operations will take overall `O(n)` time even if one of those operations may take longer.

**Example 1:**
```
Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 1, 1, false]

Explanation
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

**Labels:**
- 栈 Stack
- 简单 Easy

**思路:**

使用两个栈stack<int> s1, s2用于push, pop/peek
1. func push()
    - 如果s2不空，则将s2的所有元素push到s1中，然后将元素x入栈s1
    - 如果s2空，则将元素x直接入栈s1
2. func pop()
    - 如果s1和s2都空，则直接返回-1
    - 如果s1不空，则将s1的所有元素push到s2中，然后将s2的栈顶元素出栈并返回
    - 如果s2空，则将s2的栈顶元素出栈并返回
3. func peek()
    - 如果s1和s2都空，则直接返回-1
    - 如果s1不空，则将s1的所有元素push到s2中，然后将s2的栈顶元素返回
    - 如果s2空，则将s2的栈顶元素返回
4. func empty()
    - 返回s1.empty()和s2.empty()

**复杂度分析:**
1. 时间复杂度: O(1)， 平均时间
2. 空间复杂度: O(n), n为队列长度
3. 执行结果：通过
    - 执行用时：0 ms, 在所有 C++ 提交中击败了100%的用户
    - 内存消耗：6.6 MB, 在所有 C++ 提交中击败了98.92%的用户

**代码(C++):**
```C++
语言： cpp

class MyQueue {
    stack<int> s1;
    stack<int> s2;
public:
    /** Initialize your data structure here. */
    MyQueue() {

    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        s1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if (empty())
            return -1;
        
        int res = peek();
        s2.pop();
        return res;
    }
    
    /** Get the front element. */
    int peek() {
        if (empty())
            return -1;

        if (s2.empty()) {
            while (!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }

        return s2.top();
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return s1.empty() && s2.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```