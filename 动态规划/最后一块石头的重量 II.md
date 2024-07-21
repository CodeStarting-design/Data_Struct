# LeetCode-1049-最后一块石头的重量 II

有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。

每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

## 动态规划

此处使用动规求解，需要对问题进行转化：本质上就是将一个序列划分为两部分，两部分的差值尽可能的接近，并且需要返回两部分的差值。

先求解序列的和 `sum`，那么需要选取一个序列尽可能将大小为 `sum/2` 的背包装满的序列。

1. 动规数组：定义 `dp[i][j]` 表示从前 `i` 个物品中选取，能够将 `j` 大小的背包恰好装满
2. 数组推导：`dp[i][j]` 区别在于是否使用 `i` : `dp[i][j]=dp[i-1][j]||dp[i-1][j-stones[i]]`
3. 初始化：对于 `dp[i][0]=true` 容量为0的背包一定满 `dp[0][j]=false`
4. 遍历顺序：从上到下，从左到右遍历

```C++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int n=stones.size();
        int sum=0;
        for(int i=0;i<n;i++){
            sum+=stones[i];
        }
        int flag = sum%2,target=sum/2;
        vector<vector<bool>> dp(n+1,vector<bool>(target+1,false));
        for(int i=0;i<=n;i++) dp[i][0]=true;
        int res=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=target;j++){
                if(j>=stones[i-1])
                dp[i][j]=dp[i-1][j]||dp[i-1][j-stones[i-1]];
                else dp[i][j]=dp[i-1][j];
                res = dp[i][j]?max(res,j):res;
            }
        }
        return 2*(target-res)+flag;
    }
};
```

滚动数组优化：

```C++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int n=stones.size();
        int sum=0;
        for(int i=0;i<n;i++){
            sum+=stones[i];
        }
        int flag = sum%2,target=sum/2;
        vector<bool> dp(target+1,false);
        dp[0]=true;
        int res=0;
        for(int i=1;i<=n;i++){
            for(int j=target;j>=stones[i-1];j--){
                dp[j]=dp[j]||dp[j-stones[i-1]];
                res=dp[j]?max(res,j):res;
            }
        }
        return 2*(target-res)+flag;
    }
};
```
