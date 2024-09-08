# LeetCode-47-全排列 II

给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

## 回溯法

回溯的基本原理：对于排列中的位置idx，放入相应的元素，放过的数组不重复放入，需要记录那些元素使用过。

回溯的重点是如何处理重复的排列，本质上就是对于位置idx，数值为a的放过一次后，第二次再放入时需要跳过。为了实现这样的方式，首先将整个数组进行排序，然后向idx处插入元素，如果插入的元素与idx-1处相同，并且idx-1处的元素没有使用（此处没有使用过，说明在之前的循环中当前位置是使用过的），直接跳过。

```C++
class Solution {
public:
    vector<vector<int>> res;
    vector<int> tmp;
    vector<bool> vis;
    void backTrack(vector<int>nums,int idx){
        if(idx==nums.size()){
            res.push_back(tmp);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(!vis[i]){
                if(i!=0&&nums[i]==nums[i-1]&&!vis[i-1]) continue;
                vis[i]=true;
                tmp.push_back(nums[i]);
                backTrack(nums,idx+1);
                tmp.pop_back();
                vis[i]=false;
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        int n=nums.size();
        vis = vector<bool> (n,false);
        sort(nums.begin(),nums.end());
        backTrack(nums,0);
        return res;
    }
};
```
