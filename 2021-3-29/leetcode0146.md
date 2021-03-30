####  [LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)

***

**题目描述**

运用你所掌握的数据结构，设计和实现一个 `LRU (最近最少使用) 缓存机制`。

实现`LRUCache`类：

- `LRUCache(int capacity)` 以正整数作为容量 `capacity` 初始化 `LRU` 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value) `如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

**进阶**：你是否可以在 `O(1)` 时间复杂度内完成这两种操作？

示例：

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

提示：

- `1 <= capacity <= 3000`
- `0 <= key <= 3000`
- `0 <= value <= 104`
- 最多调用 3 * $10^4$ 次 `get` 和 `put`

***

**我的答案**

```cpp
class LRUCache {
public:
    LRUCache(int capacity) {
        _capacity = capacity;
    }
    
    int get(int key) {
        if(_cache.count(key)){
            auto iter = find(_orderList.begin(), _orderList.end(), key);
            _orderList.erase(iter);
            _orderList.insert(_orderList.begin(), key);
            return _cache[key];
        } else {
            return -1;
        }
    }
    
    void put(int key, int value) {
        if(_cache.count(key)) {
            _cache[key] = value;
            auto iter = find(_orderList.begin(), _orderList.end(), key);
            _orderList.erase(iter);
            _orderList.insert(_orderList.begin(), key);
            return;
        } 
        if(_cache.size()<_capacity) {
            _orderList.insert(_orderList.begin(), key);
            _cache[key] = value;
        } else {
            int val = _orderList.back();
            _orderList.erase(_orderList.end()-1);
            _cache.erase(val);

            _cache[key] =value;
            _orderList.insert(_orderList.begin(), key);
        }
    }

private:
    unordered_map<int, int> _cache;
    int _capacity;
    vector<int> _orderList;
};
```

时间复杂度：O(n)  空间复杂度O(n)

`LRU机制`：当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。这里的使用包括修改添加。

所以需要记录元素访问先后时间，同时要记录值和容量。使用unordered_map记录值，减少访问添加时间。

**双链表+哈希表**

```cpp
struct TwoWayNode {
    int _key{0};
    int _val{0};
    TwoWayNode* pre{nullptr};
    TwoWayNode* next{nullptr};

    TwoWayNode(){}
    TwoWayNode(int key, int val) : _key(key), _val(val){}
};

class LRUCache {
public:
    LRUCache(int capacity) {
        _capacity = capacity;
        _head = new TwoWayNode(-1,-1);
        _back = new TwoWayNode(-1, -1);
        _head->next = _back;
        _back->pre = _head;
    }
    
    int get(int key) {
        if(_cache.count(key)) {
            TwoWayNode* node = _cache[key];
            removeNode(node);
            addNode(node);
            return node->_val;
        } else {
            return -1;
        }
    }
    
    void put(int key, int value) {
        if(_cache.count(key)) {
            TwoWayNode* node = _cache[key];
            removeNode(node);
            node->_val = value;
            addNode(node);
        } else if(_cache.size() < _capacity) {
            TwoWayNode* node = new TwoWayNode(key, value);
            addNode(node);
            _cache[key] = node;
        } else if(_cache.size() >= _capacity) {
            TwoWayNode* node = _back->pre;
            removeNode(node);
            _cache.erase(node->_key);

            node = new TwoWayNode(key, value);
            addNode(node);
            _cache[key] = node; 
        }
    }

private:
    void addNode(TwoWayNode* node) {
        node->next = _head->next;
        node->pre = _head;
        _head->next->pre = node;
        _head->next = node;
    }

    void removeNode(TwoWayNode* node) {
        node->pre->next =node->next;
        node->next->pre =node->pre;
    }

private:
    int _capacity{0};
    TwoWayNode* _head{nullptr};
    TwoWayNode* _back{nullptr};
    unordered_map<int, TwoWayNode*> _cache;
};
```

时间复杂度为O(n)，空间复杂度为 O(1)

借用了双链表的插入和删除时间复杂度都是O(1)的特点