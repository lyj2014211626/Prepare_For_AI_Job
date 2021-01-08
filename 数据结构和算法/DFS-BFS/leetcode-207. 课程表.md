- [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4484571.html)
- [官方解法](https://leetcode-cn.com/problems/course-schedule/solution/course-schedule-tuo-bu-pai-xu-bfsdfsliang-chong-fa/)
- 解法一：拓扑排序-BFS
    + 用一个队列保存现有的入度为0的节点，循环跳出的条件是队列为空，没有为0的节点；再用一个入度数组保存每个节点入度，每次都将直接相连的节点的度减一，并将入度为0的节点放入队列中。
    + 也是BFS的解法
    ```C++
    class Solution {
    public:
        bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
            vector<vector<int>> adj_list(numCourses, vector<int>());//邻接链表
            vector<int> in_degree(numCourses, 0);//每个节点的入度数组
            for(auto item : prerequisites)
            {
                adj_list[item[1]].push_back(item[0]);
                ++in_degree[item[0]];
            }
            queue<int> qe;
            for(int i = 0; i < numCourses; ++i)
            {
                if(in_degree[i] == 0)
                    qe.push(i);
            }
            while(!qe.empty())
            {
                int t = qe.front();
                qe.pop();
                for(auto adj : adj_list[t])
                {
                    --in_degree[adj];
                    if(in_degree[adj] == 0)
                        qe.push(adj);
                }
            }
            for(int i = 0; i < numCourses; ++i)
            {
                if(in_degree[i] != 0)
                    return false;
            }
            return true;
        }
    };
    ```

- DFS
    + 下面来看 DFS 的解法，也需要建立有向图，还是用二维数组来建立，和 BFS 不同的是，像现在需要一个一维数组 visit 来记录访问状态，这里有三种状态，0表示还未访问过，1表示已经访问了，-1 表示有冲突。大体思路是，先建立好有向图，然后从第一个门课开始，找其可构成哪门课，暂时将当前课程标记为已访问，然后对新得到的课程调用 DFS 递归，直到出现新的课程已经访问过了，则返回 false，没有冲突的话返回 true，然后把标记为已访问的课程改为未访问。代码如下：
    ```C++
    class Solution {
    public:
        bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
            vector<vector<int>> adj_list(numCourses, vector<int>());
            vector<int> visit(numCourses, 0);
            for(auto item : prerequisites)
            {
                adj_list[item[1]].push_back(item[0]);
            }
            for(int i = 0; i < numCourses; ++i)
            {
                if(!cursion(i, adj_list, visit))
                    return false;
            }
            return true;
            
        }
        
        bool cursion(int node, vector<vector<int>>& adj_list, vector<int>& visit)
        {
            if(visit[node] == -1)
            {
                return false;
            }
            if(visit[node] == 1)
            {
                return true;
            }
            visit[node] = -1;
            for(auto it : adj_list[node])
            {
                if(!cursion(it, adj_list, visit))
                    return false;
            }
            visit[node] = 1;
            return true;
        }
    };
    ```