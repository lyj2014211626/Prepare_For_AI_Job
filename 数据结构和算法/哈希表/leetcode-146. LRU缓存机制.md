- [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4587511.html)
- [官方解法](https://leetcode-cn.com/problems/lru-cache/solution/lruhuan-cun-ji-zhi-by-leetcode-solution/)
- 解法：双链接 + 哈希表
    ``` C++
    class LRUCache {
    public:
        LRUCache(int capacity) {
            cnt = capacity;
        }
        
        int get(int key) {
            auto it = mp.find(key);
            if(it == mp.end())
                return -1;
            else
            {
                l.splice(l.begin(), l, it->second);
                return it->second->second;
            }
        }
        
        void put(int key, int value) {
            auto it  = mp.find(key);
            if(it != mp.end())
                l.erase(it->second);
            l.push_front(make_pair(key, value));
            mp[key] = l.begin();
            if(mp.size() > cnt)
            {
                int t = l.rbegin()->first;
                l.pop_back();
                mp.erase(t);
            }
        }
    private:
        int cnt = 0;
        list<pair<int, int>> l;
        unordered_map<int, list<pair<int, int>>::iterator> mp;
    };

    /**
     * Your LRUCache object will be instantiated and called as such:
     * LRUCache* obj = new LRUCache(capacity);
     * int param_1 = obj->get(key);
     * obj->put(key,value);
     */
    ```