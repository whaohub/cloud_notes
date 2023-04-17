[toc]

## 16 LRU 缓存

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

 

**示例：**

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

 

**提示：**

- `1 <= capacity <= 3000`
- `0 <= key <= 10000`
- `0 <= value <= 105`



```cpp
class LRUCache {
private:
    list<pair<int, int>> cache;
    unordered_map<int, list<pair<int,int>>::iterator> hash;
    int cap;
public:
    LRUCache(int capacity):cap(capacity) {

    }
    
    int get(int key) 
    {
        if(hash.find(key) == hash.end())
        {
            return -1;
        }

        pair<int, int>node = *hash[key];
        cache.erase(hash[key]);
        cache.push_front(node);
        hash[key] = cache.begin();
        return node.second;
    }
    
    void put(int key, int value) 
    {
        if(hash.count(key))
        {
            cache.erase(hash[key]);
        }
        else
        {
            if(cap == cache.size())
            {
                hash.erase(cache.back().first);
                cache.pop_back();
            }
        }

        auto newnode = std::make_pair(key, value);
        cache.push_front(newnode);
        hash[key]=cache.begin();
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```





## 17 . 前 K 个高频元素

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

 

**示例 1:**

```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

**示例 2:**

```
输入: nums = [1], k = 1
输出: [1]
```

 

**提示：**

- `1 <= nums.length <= 105`
- `k` 的取值范围是 `[1, 数组中不相同的元素的个数]`
- 题目数据保证答案唯一，换句话说，数组中前 `k` 个高频元素的集合是唯一的

 **进阶：**你所设计算法的时间复杂度 **必须** 优于 `O(n log n)` ，其中 `n` 是数组大小。



```CPP
class Solution 
{
public:
    vector<int> topKFrequent(vector<int>& nums, int k) 
    {
        struct mycompare
        {
            bool operator()(pair<int, int>& a, pair<int,int>&b)
            {
                return a.second>b.second;
            };
        };

        unordered_map<int,int>map;

        //统计元素出现次数
        for(auto &val:nums)
        {
            map[val]++;
        }

        priority_queue<pair<int, int>, vector<pair<int, int>>, mycompare> min_heap;

        for(auto &val:map)
        {
            min_heap.push(val);
            if(min_heap.size() > k)
            {
                min_heap.pop();
            }
        }

        std::vector<int> res;
        while(!min_heap.empty())
        {
            res.push_back(min_heap.top().first);
            min_heap.pop();
        }
        return res;
    }
};
```

思路:https://leetcode.cn/problems/top-k-frequent-elements/solutions/1283998/c-xiao-bai-you-hao-you-xian-dui-lie-de-j-53ay/

- 时间复杂度：O(nlogk)，n表示数组的长度。首先，遍历一遍数组统计元素的频率，这一系列操作的时间复杂度是 O(n)；接着，遍历用于存储元素频率的 map，如果元素的频率大于最小堆中顶部的元素，则将顶部的元素删除并将该元素加入堆中，这里维护堆的数目是 k，所以这一系列操作的时间复杂度是 O(nlogk)的；因此，总的时间复杂度是 O(nlog⁡k)。
- 空间复杂度：O(n)，最坏情况下（每个元素都不同），map 需要存储 nnn 个键值对，优先队列需要存储 kkk 个元素，因此，空间复杂度是 O(n)。

