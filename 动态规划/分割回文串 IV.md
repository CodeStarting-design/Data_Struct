# LeetCode-1745-分割回文串 IV

给你一个字符串 s ，如果可以将它分割成三个 非空 回文子字符串，那么返回 true ，否则返回 false 。

当一个字符串正着读和反着读是一模一样的，就称其为 回文字符串 。

## 动态规划

问题本质上就是能否恰好划分为k个。

1. 动规数组：定义 `dp[n][k]` 能否将前n个字符，划分为k个回文子串。
2. 数组推导：对于 `dp[i][j]` 能否将前 `i` 个字符划分为 `j` 份，此时只需要关注，第 `j` 份要如何划分，`s[m-1,...,i-1]` 作为一份，那么只需要判断 `dp[m-1][j-1]` 是否为真即可。
3. 初始化：对于 `dp[i][i]` 为true，其他全部初始化为false
4. 遍历顺序：从上到下，从左至右逐行遍历

记录：使用一个二维数组记录 `s[i,...,j]` 是否为回文子串，使用动规初始化

```c++
class Solution {
public:
    void backString(string s,vector<vector<bool>> &back){
        int n=s.size();
        for(int i=n-2;i>-1;i--){
            for(int j=i+1;j<n;j++){
                if(j==i+1){
                    back[i][j]=s[i]==s[j];
                }else{
                    back[i][j]=s[i]==s[j]?back[i+1][j-1]:false;
                }
            }
        }
    }
    bool checkPartitioning(string s) {
        int n=s.size();
        vector<vector<bool>> dp(n+1,vector<bool>(4,false));
        for(int i=0;i<4;i++) dp[i][i]=true;
        vector<vector<bool>> back(n,vector<bool>(n,false));
        for(int i=0;i<n;i++) back[i][i]=true;
        backString(s,back);
        for(int i=1;i<=n;i++){
            for(int j=1;j<=i&&j<=3;j++){
                for(int m=j;m<=i;m++){
                    dp[i][j] = dp[i][j]|| (dp[m-1][j-1]&&back[m-1][i-1]);
                }
            }
        }
        return dp[n][3];
    }
};
```
