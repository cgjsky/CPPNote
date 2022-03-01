对整个空间进行搜索，都是树形结构，搜索顺序不一样

DFS：

对每个结点尽可能达到最深，回溯时，并不是回溯到初始结点，而是对回溯的每个结点深搜 O(h)，stack 回溯+剪枝

BFS：

将该结点的所有邻居结点进行搜索，每层进行，一般需要辅助队列 O(2^h)

DFS空间更有优势，但BFS可以算“最短路”

```c++
//BFS模版
queue<> q;
//q不空
while(!q.empty()){
  //拿头
  auto x=q.front;
  q.pop();
  //拓展
  for( ){
    if(...){
      q.push();
    }
  }
}
```



# 回溯的三种问题

## 1.子集

```c++
//问题很简单，输入一个不包含重复数字的数组，要求算法输出这些数字的所有子集
//由于子集是对于每个结点都要选取，所以不需要判断是否path.size()==n；
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
    void dfs(int cur,vector<int> nums){
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
            dfs(i+1,nums);
            path.pop_back();
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        dfs(0,nums);
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

## 二维dfs

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

# BFS与DFS

数据结构   空间

stack        O(h	   不具有最短性

queue 	  O(2^h)  最短路

## DFS 回溯➕剪枝

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

## BFS 迷宫问题

一定要注意迷宫问题的二维矩阵不一定长宽都是n，可能一个n，一个m，不要再WA了

```c++
#include<iostream>
#include<queue>
#include<cstring>
using namespace std;
const int N=110;
int n,m;
int g[N][N],d[N][N];
int dir[4][2]={{-1,0},{1,0},{0,-1},{0,1}};
void bfs( ){
    queue<pair<int, int>> q;
    memset(d,-1,sizeof(d));
    q.push({0,0});
    d[0][0]=0;
    while(!q.empty()){
        auto [x,y]=q.front();
        q.pop();
        for(int i=0;i<4;i++){
            int dx=x+dir[i][0],dy=y+dir[i][1];
            if(g[dx][dy]==0&&d[dx][dy]==-1&&dx>=0&&dx<n&&dy>=0&&dy<m){
                d[dx][dy]=d[x][y]+1;
                q.push({dx,dy});
            }
        }
    }
    cout<<d[n-1][m-1]<<endl;
}
int main(){
    cin>>n>>m;
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            int x;
            cin>>x;
            g[i][j]=x;
        }
    }
    bfs();
    return 0;
}
```

