# LeetCode-337-打家劫舍 III

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 root 。

除了 root 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 两个直接相连的房子在同一天晚上被打劫 ，房屋将自动报警。

给定二叉树的 root 。返回 在不触动警报的情况下 ，小偷能够盗取的最高金额 。

## 动态规划

此处是在一棵二叉树的结构上进行dp，就不能使用数组的方式去处理了。

1. 动规数组：`dp[i][0]` 表示第i个节点不偷，`dp[i][1]` 表示第i个节点偷
2. 状态转移方程：`dp[i][0] = max(dp[i-1][0], dp[i-1][1])` `dp[i][1]=dp[i-1][0]+root.val`

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int rob(TreeNode* root) {
        return max(robTree(root,true),robTree(root,false));
    }
    int robTree(TreeNode* root ,bool flag){
        if (root){
            if (flag){
                return robTree(root->left,false)+robTree(root->right,false)+root->val;
            }else{
                return max(robTree(root->left,false),robTree(root->left,true))+max(robTree(root->right,false),robTree(root->right,true));
            }
        }
        return 0;
    }
};
```

上述代码会出现超时，主要原因在于 `robTree(root,true),robTree(root,false)` 中存在大量的重复计算，那么这些重复的计算本上就可以使用动规数组进行保存，本质上就类似于记忆话搜索。

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    unordered_map<TreeNode*,pair<int,int>> nodeMap;
    int rob(TreeNode* root) {
        return max(robTree(root,true),robTree(root,false));
    }
    int robTree(TreeNode* root ,bool flag){
        if (root){
            if (flag){
                int leftFalse,rightFalse;
                if(nodeMap.find(root->left)==nodeMap.end()){
                    nodeMap[root->left].first=robTree(root->left,false);
                    nodeMap[root->left].second=robTree(root->left,true);
                    
                }
                if(nodeMap.find(root->right)==nodeMap.end()){
                    nodeMap[root->right].first=robTree(root->right,false);
                    nodeMap[root->right].second=robTree(root->right,true);
                   
                }
                leftFalse=nodeMap[root->left].first;
                rightFalse=nodeMap[root->right].first;
                return leftFalse+rightFalse+root->val;
            }else{
                int leftVal,rightVal;
                 if(nodeMap.find(root->left)==nodeMap.end()){
                    nodeMap[root->left].first=robTree(root->left,false);
                    nodeMap[root->left].second=robTree(root->left,true);
                    
                }
                if(nodeMap.find(root->right)==nodeMap.end()){
                    nodeMap[root->right].first=robTree(root->right,false);
                    nodeMap[root->right].second=robTree(root->right,true);
                   
                }
                leftVal=max(nodeMap[root->left].first,nodeMap[root->left].second);
                rightVal=max(nodeMap[root->right].first,nodeMap[root->right].second);
                return leftVal+rightVal;
            }
        }
        return 0;
    }
};
```

不使用hash进行记录的方法：在递归函数进行调用时直接返回两种情形：

```C++
struct SubtreeStatus {
    int selected;
    int notSelected;
};

class Solution {
public:
    SubtreeStatus dfs(TreeNode* node) {
        if (!node) {
            return {0, 0};
        }
        auto l = dfs(node->left);
        auto r = dfs(node->right);
        int selected = node->val + l.notSelected + r.notSelected;
        int notSelected = max(l.selected, l.notSelected) + max(r.selected, r.notSelected);
        return {selected, notSelected};
    }

    int rob(TreeNode* root) {
        auto rootStatus = dfs(root);
        return max(rootStatus.selected, rootStatus.notSelected);
    }
};
```
