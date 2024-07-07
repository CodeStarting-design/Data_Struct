# LeetCode 123 买卖股票的最佳时机 III

给定一个数组，它的第i个元素是一支给定的股票在第i天的价格。设计一个算法来计算你所能获取的最大利润。你最多可以完成两笔交易。

此处是限定为可以完成两笔交易，可以进一步限制为可以进行k笔交易

## 动态规划

此处的重点是动态规划数组的定义。

1. 动规数组的定义：`dp[n][5]` 表示：
   1. `dp[i][0]` 表示第 `i` 对股票未进行任何操作的最大利润，一定为0
   2. `dp[i][1]` 表示第 `i` 第一次持有股票的最大利润
   3. `dp[i][2]` 表示第 `i` 第一次卖出股票的最大利润
   4. `dp[i][3]` 表示第 `i` 第二次持有股票的最大利润
   5. `dp[i][4]` 表示第 `i` 第二次卖出股票的最大利润
2. 动规数组的推导：
   1. `dp[i][1]=max(dp[i-1][1],dp[i][0]-prices[i])`
   2. `dp[i][2]=max(dp[i-1][1]+prices[i],dp[i-1][2])`
   3. `dp[i][3]=max(dp[i-1][3],dp[i][2]-prices[i])`
   4. `dp[i][4]=max(dp[i-1][3]+prices[i],dp[i-1][4])`
3. 初始化：对于 `dp[0][1]=-prices[0],dp[0][2]=0,dp[0][3]=-prices[0],dp[0][4]=0`
4. 遍历顺序：首先从上到下，再从左到右遍历

在上述基础砂锅可以优化为5长度为2的数组

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        if (n==1) return 0;
        vector<vector<int>> dp(2,vector<int>(5,0));
        dp[0][1]=-prices[0],dp[0][3]=-prices[0];
        for(int i=1;i<n;i++){
            dp[i%2][1]=max(dp[(i-1)%2][1],dp[i%2][0]-prices[i]);
            dp[i%2][2]=max(dp[(i-1)%2][1]+prices[i],dp[(i-1)%2][2]);
            dp[i%2][3]=max(dp[(i-1)%2][3],dp[i%2][2]-prices[i]);
            dp[i%2][4]=max(dp[(i-1)%2][3]+prices[i],dp[(i-1)%2][4]);
        }
        return dp[(n-1)%2][4];
    }
};
```
