- [295. 数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4896673.html)
- [官方解法](https://leetcode-cn.com/problems/find-median-from-data-stream/solution/shu-ju-liu-de-zhong-wei-shu-by-leetcode/)
- 解法一：双堆法
    + 我们将中位数左边的数保存在大顶堆中，右边的数保存在小顶堆中。这样我们可以在O(1) 时间内得到中位数。
    + 这里让左边的大顶堆的长度大于等于右边的小顶堆。当为偶数的时候，大顶堆和小顶堆的长度一样，奇数大顶堆大于小顶堆。
    ```C++
    class MedianFinder {
    private:
        priority_queue<int> max_pq;//大顶堆
        priority_queue<int, vector<int>, greater<int>> min_pq;//小顶堆
    public:
        /** initialize your data structure here. */
        MedianFinder() {

        }
        
        void addNum(int num) {
            max_pq.push(num);   //先插入大顶堆，调整堆的顺序。
            min_pq.push(max_pq.top());//在将大顶堆的最大值放到小顶堆中。调节小顶堆。
            max_pq.pop();
            if(max_pq.size() < min_pq.size())
            {
                max_pq.push(min_pq.top());
                min_pq.pop();
            }
        }
        
        double findMedian() {
            return max_pq.size() > min_pq.size() ? max_pq.top() : (max_pq.top() + min_pq.top()) / 2.0;
        }
    };

    /**
     * Your MedianFinder object will be instantiated and called as such:
     * MedianFinder* obj = new MedianFinder();
     * obj->addNum(num);
     * double param_2 = obj->findMedian();
     */
    ```