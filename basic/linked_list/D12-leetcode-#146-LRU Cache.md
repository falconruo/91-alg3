**\u9898\u76ee:**

146. LRU Cache

Design a data structure that follows the constraints of a **Least Recently Used (LRU) cache**.

Implement the **LRUCache** class:
- ```LRUCache(int capacity)``` Initialize the LRU cache with positive size ```capacity```.
- ```int get(int key)``` Return the value of the ```key``` if the key exists, otherwise return ```-1```.
- ```void put(int key, int value)``` Update the value of the ```key``` if the ```key``` exists. Otherwise, add the ```key-value``` pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.

**Follow up:**
Could you do ```get``` and ```put``` in ```O(1)``` time complexity?

**Example 1:**
```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

**Constraints:**
- ```1 <= capacity <= 3000```
- ```0 <= key <= 3000```
- ```0 <= value <= 104```
- At most ```3 * 104``` calls will be made to get and put.

**Labels:**
- 中等
- 链表

**思路:**

LRU(least recently used) Cache:
1. 利用双向链表来作为cache存放结点,规则如下：
    - 定义两个头尾结点head, tail
    - 尾部添加结点，evict头部结点,中间结点移动(move to tail)
    - 最近被访问的结点(get, put)放在尾部
    - 总的结点数 = capacity时, 对于新增加的结点, 首先删除head->next指向的结点, 然后将新结点添加在尾部(tail->prev->next = node)
2. 利用hashmap的特性定位对应于链表中的位置，实现get和put在O(1)的时间复杂度，hashmap定义如下:
    - key是给定的key
    - value是指向双向链表的对应结点的指针

**复杂度分析:**
1. 时间复杂度: O(1)
2. 空间复杂度: O(capacity)
3. 执行结果：通过
    - 执行用时：92 ms, 在所有 C++ 提交中击败了97.35%的用户
    - 内存消耗：38.9 MB, 在所有 C++ 提交中击败了89.10%的用户

**代码(C++):**
```C++
class LRUCache {
public:
    struct DoubleList {
        int key;
        int val;
        DoubleList* prev;
        DoubleList* next;
        DoubleList(int k, int v) : key(k), val(v), prev(NULL), next(NULL) {}
    };

    LRUCache(int capacity) {
        head = new DoubleList(-1, -1);
        tail = new DoubleList(-2, -2);

        head->next = tail;
        tail->prev = head;

        size = capacity;
    }
    
    int get(int key) {
        if (dict.empty() || !dict.count(key)) return -1;

        DoubleList* node = dict[key];

        if (dict.size() > 1) {
            delnode(node);
            add2tail(node);
        }
        return node->val;
    }
    
    void put(int key, int value) {
        // the least used node is in the head
        // the most used node is in the tail
        DoubleList* node = NULL;

        if (!dict.empty() && dict.count(key)) {
            node = dict[key];
            node->val = value;

            if (dict.size() > 1) {
                delnode(node);
                add2tail(node);
            }
        } else {
            node = new DoubleList(key, value);
            if (dict.size() >= size) {
                int k = head->next->key;
                delnode(head->next);
                dict.erase(k);
            }
            dict[key] = node;
            add2tail(node);
        }
    }
private:
    unordered_map<int, DoubleList*> dict;
    int size;
    DoubleList* head;
    DoubleList* tail;

    void add2tail(DoubleList* node) {
        tail->prev->next = node;
    
        node->prev = tail->prev;
        node->next = tail;
        tail->prev = node;
    }

    void delnode(DoubleList* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```
