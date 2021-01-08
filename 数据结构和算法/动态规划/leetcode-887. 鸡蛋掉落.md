- [887. 鸡蛋掉落](https://leetcode-cn.com/problems/super-egg-drop/)
- [参考解法](https://www.cnblogs.com/grandyang/p/11048142.html#4285408)
- [官方解法](https://leetcode-cn.com/problems/super-egg-drop/solution/dong-tai-gui-hua-zhi-jie-shi-guan-fang-ti-jie-fang/)
- 解法一：DP＋二分
    + 二分是最小取最大，就是二个直线相交的位置。
    ```C++
    class Solution {
    public:
        int superEggDrop(int K, int N) {
            vector<vector<int>> dp(K + 1, vector<int>(N + 1));
            for(int i = 1; i <= N; ++i)
                dp[1][i] = i;
            for(int i = 2; i <= K; ++i)
            {
                for(int j = 1; j <= N; ++j)
                {
                    dp[i][j] = j;
                    int left = 1;
                    int right = j;
                    while(left < right)
                    {
                        int mid = left + (right - left) / 2;
                        if(dp[i - 1][mid - 1] < dp[i][j - mid])
                            left = mid + 1;
                        else right = mid;
                    }
                    dp[i][j] = min(dp[i][j], max(dp[i - 1][left - 1], dp[i][j - left]) + 1);
                }
            }
            return dp[K][N];
        }
    };
    ```