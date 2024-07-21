# LeetCode-647. 回文子串

给你一个字符串 s ，请你统计并返回这个字符串中 回文子串 的数目。

回文字符串 是正着读和倒过来读一样的字符串。

子字符串 是字符串中的由连续字符组成的一个序列。

## 中心扩散法

最简单的动规就是判断 `dp[i][j]` 是否是一个回文子串。

中心扩散的思想其实很简单，对于一个回文子串，一定存在一个中心，从这个中心点像两端扩散，每走一步都是相等的。（对于长度为偶数的回文串，中心是两个点，对于长度为奇数的实际上中心也是两个点，但是两个点相同）

```c++
class Solution {
public:
    int countString(string s,int a,int b){// 以a，b为中心寻找
        int count=0;
        while(a>-1&&b<s.size()&&s[a]==s[b]){
            count++;
            a--;
            b++;
        }
        return count;
    }
    int countSubstrings(string s) {
        int n=s.size();
        int res=0;
        for(int i=0;i<n;i++){
            res+=countString(s,i,i+1);
            res+=countString(s,i,i);
        }
        return res;
    }
};
```
