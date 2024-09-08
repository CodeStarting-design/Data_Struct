# LeetCode-698-划分为k个相等的子集

给定一个整数数组  nums 和一个正整数 k，找出是否有可能把这个数组分成 k 个非空子集，其总和都相等

## 暴力回溯

```C++
class Solution {
public:
    vector<int> tmp;
    bool backTracking(vector<int> nums,int idx,int k, int target){
        if(idx==nums.size()){
            for(int i=0;i<k;i++){
                if(tmp[i]!=target) return false;
            }
            return true;
        }
        bool flag=false;
        for(int i=0;i<k;i++){
            if(tmp[i]<target&&tmp[i]+nums[idx]<=target){
                tmp[i]+=nums[idx];
                flag=backTracking(nums,idx+1,k,target);
                if(flag) return true;
                tmp[i]-=nums[idx];
            }
        }
        return false;
    }
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum=0;
        for(int i=0;i<nums.size();i++){
            sum+=nums[i];
        }
        if(sum%k) return false;
        int target=sum/k;
        tmp=vector<int>(k,0);
        return backTracking(nums,0,k,target);
    }
}; 
```

在上述基础上做剪枝：

```C++
class Solution {
public:
    vector<int> tmp;
    bool backTracking(vector<int> nums,int idx,int k, int target){
        if(idx==-1){
            for(int i=0;i<k;i++){
                if(tmp[i]!=target) return false;
            }
            return true;
        }
        bool flag=false;
        for(int i=0;i<k;i++){
            if(tmp[i]+nums[idx]> target || (i!=0&&tmp[i]==tmp[i-1]) ){
                continue;
            }
            tmp[i]+=nums[idx];
            flag=backTracking(nums,idx-1,k,target);
            if(flag) return true;
            tmp[i]-=nums[idx];
        }
        return false;
    }
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum=0;
        for(int i=0;i<nums.size();i++){
            sum+=nums[i];
        }
        if(sum%k) return false;
        int target=sum/k;
        tmp=vector<int>(k,0);
        sort(nums.begin(),nums.end());
        return backTracking(nums,nums.size()-1,k,target);
    }
};
```
