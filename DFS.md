

# 回溯

## 框架

解决一个回溯问题，就是解决一个决策树的遍历问题，通过暴力不断寻找ans

1. 路径，已经作出的选择
2. 选择列表，目前可以做的选择
3. 结束条件，在情况下return

```c++
//框架
result = []
void dfs(){
  	if 满足结束条件:
  		result.add(路径)
      return
    for 选择 in 选择列表：
     	做出选择//选择当前节点
      dfs(路径，选择列表)//进入下一层
      撤销选择//返回上一层，并通过for进入兄弟节点
}
```



## 1.子集

```c++
//问题很简单，输入一个不包含重复数字的数组，要求算法输出这些数字的所有子集
//由于子集是对于每个结点都要选取，所以不需要判断是否path.size()==n；
1.path,对于每个path，直接加入ans
2.选择列表，从idx到nums.size()
3.撤回操作
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    void dfs(int cur,vector<int>& nums){
        ans.push_back(path);//把每个分支的结点都加入进来
        for(int i=cur;i<nums.size();i++){
            path.push_back(nums[i]);
            dfs(i+1,nums);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        dfs(0,nums);
        return ans;
    }
};
```

```c++
//带重复元素
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    void dfs(int cur,vector<int>& nums,vector<int>& used){
        ans.push_back(path);
        for(int i=cur;i<nums.size();i++){
          //剪枝,当used[i-1]==0时，nums[i-1]肯定已经被使用过，这次轮到nums[i]，由于nums[i-1]=nums[i]，所以把nums[i]这一枝剪掉
            if(i&&nums[i]==nums[i-1]&&used[i-1]==0) continue;
            path.push_back(nums[i]);
            used[i]=1;
            dfs(i+1,nums,used);
            path.pop_back();
            used[i]=0;
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        int n=nums.size();
        vector<int> used(n);
        sort(nums.begin(),nums.end());
        dfs(0,nums,used);
        return ans;
    }
};
```

## 2.排列

```c++
//输入一个不包含重复数字的数组 nums，返回这些数字的全部排列。
//对于全排列问题，要注意for循环是从0开始
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    void dfs(vector<int> nums){
      //path.size判断
        if(path.size()==nums.size()){
            ans.push_back(path);
            return;
        }
      //for从0开始循环
        for(int i=0;i<nums.size();i++){
          //也可以通过st数组来判断nums[i]是否被使用过
            if(find(path.begin(), path.end(), nums[i]) != path.end()) continue;
            path.push_back(nums[i]);
            dfs(nums);
            path.pop_back();
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        dfs(nums);
        return ans;
    }
};
```

## 3.组合

```c++
//输入两个数字 n, k，算法输出 [1..n] 中 k 个数字的所有组合。
相对于子集问题，仅仅是多了一个k值判断
class Solution {
    vector<vector<int>> ans;
    vector<int> r;
public:
    void dfs(int n,int k,int idx){
      //判断是否读出
        if(r.size()==k){
            ans.push_back(r);
            return ;
        }
        for(int i=idx;i<=n;i++){
            r.push_back(i);
            dfs(n,k,i+1);
            r.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        dfs(n,k,1);
        return ans;
    }
};
```

# 回溯➕剪枝

每个DFS一定对应一个搜索树,最重要的是考虑顺序

回溯的时候一定要进行恢复，不然等着你妈打你

### 八皇后问题

```c++
//第一种搜索顺序，每一列，对角线以及反对角线不可有重复
#include<iostream>
#include<cstring>
using namespace std;
const int N=50;
int n;
int col[N],dg[N],udg[N];
char g[N][N];
void dfs(int u){
    if(u==n){
        for(int i=0;i<n;i++) puts(g[i]);
        cout<<endl;
        return ;
    }
    for(int i=0;i<n;i++){
        if(!col[i]&&!dg[u+i]&&!udg[n+i-u]){
            g[u][i]='Q';
            col[i]=dg[u+i]=udg[n+i-u]=1;
            dfs(u+1);
            g[u][i]='.';
            col[i]=dg[u+i]=udg[n+i-u]=0;
        }
    }
}
int main(){
    cin>>n;
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            g[i][j]='.';
        }
    }
    dfs(0);
    return 0;
}
```

```c++
//第二种搜索顺序，针对每一个皇后数量进行搜索，每一个格子有放不放两种选择，总共有2^(n^2)种选择，当u==n，即皇后数量为n时，打印结果
```

# 回溯➕记忆化

```c++
单词拆分
//给你一个字符串 s 和一个字符串列表 wordDict 作为字典。请你判断是否可以利用字典中出现的单词拼接出s
//注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。
普通暴力会超时，需要加记忆化，对于目标s的idx下标不能在字典里找到后续拼接的，把idx存入set
		set<int> se;
		bool dfs(string& s,vector<string>& wordDict, int idx){
        if(idx==s.size()) return 1;
        for(int i=idx+1;i<=s.size();i++){
            if(se.count(i)) continue;
            if(find(wordDict.begin(),wordDict.end(),s.substr(idx,i-idx))!=wordDict.end()){
                if(dfs(s,wordDict,i)) return 1;
                se.insert(idx);
            }
        }
        return 0;
    }
    bool wordBreak(string s, vector<string>& wordDict) {
        return dfs(s,wordDict,0);
    }
```



# 二维dfs

```c++
//搜索过程如下：
int dx[4] = {-1,0,1,0}, dy[4] = {0,1,0,-1}; //方向数组
bool dfs(vector<vector<char>>& board, string& word,int u,int x,int y)
    {
        if(board[x][y] != word[u]) return false;
        if(u == word.size() - 1)   return true;
    		//相当于used数组
        char t = board[x][y];
        board[x][y] = '.';
       //每个方向深度搜索
        for(int i = 0; i < 4; i++)
        {
            int a = x + dx[i], b = y + dy[i];
            //出界或者走到已经搜索过的位置
            if(a < 0 || a >= board.size() || b < 0 || b >= board[0].size() || board[a][b] == '.')  continue;
            if(dfs(board,word,u+1,a,b)) return true;
        }
        board[x][y] = t;
        return false;
}
```

# 
