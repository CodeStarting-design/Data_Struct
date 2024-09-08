# LeetCode-680-验证回文串 II

给你一个字符串 s，最多 可以从中删除一个字符。

请你判断 s 是否能成为回文字符串：如果能，返回 true ；否则，返回 false 。

## 双指针

首先是双指针判断，是否是回文串，当出现了两个指针所指字符不一致的情况时，那次此处就一定需要删除，有两种选择，删除left或者删除right。

```C++
class Solution {
public:
    bool isRound(string s,int l,int r){
        while(l<=r){
            if(s[l]!=s[r]) return false;
            l++,r--;
        }
        return true;
    }
    bool validPalindrome(string s) {
        int l=0,r=s.size()-1;
        while(l<=r){
            if(s[l]!=s[r]){
                return isRound(s,l+1,r)||isRound(s,l,r-1);
            }
            l++,r--;
        }
        return true;
    }
};
```
