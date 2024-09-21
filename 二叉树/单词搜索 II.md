# LeetCode-212-单词搜索

给定一个 m x n 二维字符网格 board 和一个单词（字符串）列表 words， 返回所有二维网格上的单词 。

单词必须按照字母顺序，通过 相邻的单元格 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

## 暴力回溯

对于每一个起点，遍历每一个单词。

```C++
class Solution {
public:
    vector<vector<bool>> vis;
    bool dfs(vector<vector<char>>& board,int i,int j,string word){
        
    }
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        int n=board.size(),m=board.front().size();
        vis=vector<vector<bool>>(n,vector<bool>(m,false));
    }
};
```

## 字典树

此处的基本思路是先构造字典树，在图中再进行遍历，在遍历的过程中判断是否是一个单词，若是存在的单词，那么就添加到结果集中。

此处在前缀树的构造上存在巧妙之处，使用的结构体如下：

```C++
struct Tire
{
    string word;
    unordered_map<char, Tire *> child;
};
```

此处保存 `word` 是为了当出现了一个完整的单词时，可以直接保存到结果集中。

相应的前缀树的插入过程为：

```C++
void InsertTire(Tire *root, string word)
{
    for (int i = 0; i < word.size(); i++)
    {
        if (root->child.count(word[i]) == 0)
        {
            root->child[word[i]] = new Tire();
        }
        root = root->child[word[i]];
    }
    root->word = word;
}
```

完成树的构造之后，开始DFS在图中进行遍历，判断在DFS的过程中是否出现前缀树中的单词

```C++
class Solution
{
public:
    void dfs(vector<vector<char>> &board, unordered_set<string> &res, Tire *root, int i, int j)
    {
        if (root->child.count(board[i][j]) == 0)
        {
            return;
        }
        root = root->child[board[i][j]];
        if (root->word.size() > 0)
        {
            res.insert(root->word);
        }
        char temp = board[i][j];
        board[i][j] = '*';
        if (i > 0 && board[i - 1][j] != '*')
        {
            dfs(board, res, root, i - 1, j);
        }
        if (i < board.size() - 1 && board[i + 1][j] != '*')
        {
            dfs(board, res, root, i + 1, j);
        }
        if (j > 0 && board[i][j - 1] != '*')
        {
            dfs(board, res, root, i, j - 1);
        }
        if (j < board[0].size() - 1 && board[i][j + 1] != '*')
        {
            dfs(board, res, root, i, j + 1);
        }
        board[i][j] = temp;
    }
    vector<string> findWords(vector<vector<char>> &board, vector<string> &words)
    {
        Tire *root = new Tire();
        for (auto &word : words)
        {
            InsertTire(root, word);
        }
        unordered_set<string> res;
        for (int i = 0; i < board.size(); i++)
        {
            for (int j = 0; j < board[0].size(); j++)
            {
                dfs(board, res, root, i, j);
            }
        }
        return vector<string>(res.begin(), res.end());
    }
};
```
