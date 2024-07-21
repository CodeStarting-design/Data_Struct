# LeetCode-132-分割回文串 II

给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是回文串。

返回符合要求的最少分割次数 。

## 动态规划

1. 动规数组： `dp[i][j]` 表示使得 `s[i,...,j]` 为回文串所需的最少的分割次数。
2. 数组推导：对于 `dp[i][j]` 首先判断自身是不是回文，如果不是那么只需要分为两部分 `dp[i][j]=min(dp[i][k]+dp[k+1][j]+1)`
3. 初始化：对于 `dp[i][i]=0` 对于 `dp[i][j],i>j` 全部初始化为n
4. 遍历顺序，从下往上，从左往右逐行遍历。  

```c++
class Solution {
public:
    int minCut(string s) {
        int n=s.size();
        vector<vector<int>> dp(n,vector<int>(n,n));
        for(int i=0;i<n;i++) dp[i][i]=0;
        for(int i=n-2;i>-1;i--){
            for(int j=i+1;j<n;j++){
                if(s[i]==s[j]&& (dp[i+1][j-1]==0||i+1>=j-1)){
                    dp[i][j]=0;
                }else{
                    for(int k=i;k<=j-1;k++){
                        dp[i][j]=min(dp[i][j],dp[i][k]+dp[k+1][j]+1);
                    }
                }
            }
        }
        return dp[0][n-1];
    }
}; 
```

这样的处理方式将会超出时间限制。

1. 数组定义：`dp[i]` 表示使得 `0,...,i` 为回文串所需的最少的分割次数。
2. 数组推导：对于 `dp[i]` 首先判断自身是不是回文，如果不是那么从后面截取一段回文子串 `s[j,...,i]` 那么 `dp[i]=min(dp[i],dp[j-1]+1)`，为什么直接截取 `s[j,...,i]` 是回文串的情形，因为对于后半部分不是回文串，本质上还要划分，划分的可以加归纳到前一部分，最后一段还是回文串。（或者单纯从要全部划分为回文串，最后一段一定就是回文串就可以理解）
3. 初始化：`dp[0]=0` 其他全部初始化为n
4. 遍历顺序：从前向后遍历

再使用一个数组记录下 `s[j,...,i]` 是不是回文串。

```c++
class Solution {
public:
    int minCut(string s) {
        int n=s.size();
        vector<vector<bool>> back(n,vector<bool>(n,false));
        for(int i=0;i<n;i++) back[i][i]=true;
        for(int i=n-2;i>-1;i--){
            for(int j=i+1;j<n;j++){
                if(s[i]==s[j]&&(back[i+1][j-1]||j==i+1)) back[i][j]=true;
            }
        }
        vector<int> dp(n,n);
        dp[0]=0;
        for(int i=1;i<n;i++){
            if(back[0][i]){
                dp[i]=0;
            }else{
                for(int j=i;j>-1;j--){
                    if(back[j][i]){
                        dp[i]=min(dp[i],dp[j-1]+1);
                    }
                }
            }
        }
        return dp[n-1];
    }
}; 
```
