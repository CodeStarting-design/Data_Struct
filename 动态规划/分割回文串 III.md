# LeetCode-1278-分割回文串 III

给你一个由小写字母组成的字符串 s，和一个整数 k。

请你按下面的要求分割字符串：

首先，你可以将 s 中的部分字符修改为其他的小写英文字母。
接着，你需要把 s 分割成 k 个非空且不相交的子串，并且每个子串都是回文串。
请返回以这种方式分割字符串所需修改的最少字符数。

注：暴力回溯的时间复杂度是O(n^k)，找到将字符串划分为K份的全部方法，然后再计算每一种修改的字符数，寻找最小值

## 动态规划

此处的要求是，先修改字符串，并且要求必须分割为K份。

1. 动规数组：`dp[i][j]` 表示表示字符串s的前i个字符分割为j个回文子串的修改的最小字符数，此处 `i>=j` `j>=1`
2. 数组推导：若是 `s[i]` 作为一份，本身就是回文子串 `dp[i][j]=dp[i-1][j-1]` ，也可以将 `s[m,...,i]` 作为一份， `dp[i][j]=dp[m-1][j-1]+a`，其中a表示的是 `s[m,...,i]` 变为回文子串需要的修改的最小字符数，此处的 `m` 有一个上限就是 `m-1>=j-1` 保证前面足够划分。
3. 数组初始化：全部初始化为s.length，`dp[i][i]=0` i=0,...,k
4. 遍历顺序：从上到下，从左到右逐行遍历。

此处在初始化时需要注意对于 `dp[i][0]` 的情形只有 `dp[0][0]` 能够初始化为0，对于 `dp[i][0]` 不能初始化为0，否则就会出现，只需要将后面的 `j` 份划分完，剩余的 `i` 份全部划分为0份，导致找不到最优

```C++
class Solution {
public:
    int minChange(string s,int start,int end){
        int count=0;
        for(;start<end;start++,end--){
            if(s[start]!=s[end]) count++;
        }
        return count;
    }
    int palindromePartition(string s, int k) {
        int n=s.size();
        if (n==k) return 0;
        vector<vector<int>> dp(n+1,vector<int>(k+1,n));
        dp[0][0]=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=i&&j<=k;j++){
                if(i!=j){
                    for(int m=j;m<=i;m++){
                        dp[i][j]=min(dp[i][j],dp[m-1][j-1]+minChange(s,m-1,i-1));
                    }
                }
                else{
                    dp[i][j]=0;
                }
            }
        }
        return dp[n][k];
    }
};
```

优化回文子串的修改次数计算，对于 `s[m,...,i]` 最少需要修改多少次，存在反复的求解。此处使用动规的方式先求解出全部的情形，存储，到时候直接使用。

```C++
class Solution {
public:
    void minChange(string s,vector<vector<int>> &change){
        int n=s.size();
        for(int i=n-2;i>-1;i--){
            for(int j=i+1;j<n;j++){
                if(j==i+1){
                    change[i][j]=s[i]==s[j]?0:1;
                }else{
                    change[i][j]=s[i]==s[j]?change[i+1][j-1]:change[i+1][j-1]+1;
                }
            }
        }
    }
    int palindromePartition(string s, int k) {
        int n=s.size();
        if (n==k) return 0;
        vector<vector<int>> dp(n+1,vector<int>(k+1,n));
        vector<vector<int>> change(n,vector<int>(n,0));
        minChange(s,change);
        dp[0][0]=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=i&&j<=k;j++){
                if(i!=j){
                    for(int m=j;m<=i;m++){
                        dp[i][j]=min(dp[i][j],dp[m-1][j-1]+change[m-1][i-1]);
                    }
                }
                else{
                    dp[i][j]=0;
                }
            }
        }
        return dp[n][k];
    }
};
```
