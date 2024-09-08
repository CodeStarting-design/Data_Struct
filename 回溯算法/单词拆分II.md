# LeetCode-140-单词拆分II

给定一个字符串 s 和一个字符串字典 wordDict ，在字符串 s 中增加空格来构建一个句子，使得句子中所有的单词都在词典中。以任意顺序 返回所有这些可能的句子。

注意：词典中的同一个单词可能在分段中被重复使用多次。

## 回溯法

对于n个字符，存在n-1个空位，在zhe这n-1个空位处加上空格。使用pre记录上一个字符的开始位置，使用idx表示处理到了哪一个字符，idx=0时，不能在0前面加空格，对于idx==n时，说明到达最后一个，此时判断pre和n-1组成的字符是否为字典中的单词。

```C++
class Solution {
public:
    vector<string> res;
    bool isInword(string a,vector<string>& wordDict){
        for(int i=0;i<wordDict.size();i++){
            if (a==wordDict[i]) return true;
        }
        return false;
    }
    void backStrack(string s,vector<string>& wordDict,int idx,int pre,string tmp){
        if(idx==s.size()){
            // 截断判断
            string str=s.substr(pre,idx-pre);
            if (isInword(str,wordDict)){
                if(pre!=0)
                res.push_back(tmp+" "+str);
                else
                res.push_back(str);
            }
            return;
        }
        // 在idx前只有加和不加两种选择
        if(idx==0||!isInword(s.substr(pre,idx-pre),wordDict)){// 第一个不能为空，或者不在集合中
            backStrack(s,wordDict,idx+1,pre,tmp);
        }else{
            backStrack(s,wordDict,idx+1,pre,tmp);// 尝试不加
            if(pre!=0)
            backStrack(s,wordDict,idx+1,idx,tmp+" "+s.substr(pre,idx-pre));// 尝试加
            else
            backStrack(s,wordDict,idx+1,idx,s.substr(pre,idx-pre));
        }
    }
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        backStrack(s,wordDict,0,0,"");
        return res;
    }
};
```

此处判断子串是否在集合中还可以优化，基于hash实现，能够进一步加快判断的过程
