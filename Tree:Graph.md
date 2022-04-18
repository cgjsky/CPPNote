涉及到图的问题，思考

入度、出度、DFS、BFS、拓扑、并查集

# 判断图是否是树

```c++
//并查集判断，
树的定义：1.无环 2.连通图
并查集判断有无环：如果连接的两边父节点相同，有环
判断是否连通：只有一个节点的根节点是自己，多个则不连通
int p[2010];
int find(int x){
  	if(p[x]!=x) p[x]=find(p[x]);
  	return p[x];
}
bool uni(int a,int b){
  	int pa=find(a);
  	int pb=find(b);
  	if(pa==pb) return false;
  	else p[pa]=pb;
  	return true;
}
bool validTree(int n,vector<vector<int>>& edges){
  	if(edges.size()!=n-1) return false;
  	for(int i=0;i<n;i++) p[i]=i;
  	//判断是否有环
  	for(auto& e:edges){
      	if(uni(e[0],e[1])==false) return false;
    }
  	//判断是否有一个根节点
  	int cnt=0;
  	for(int i=0;i<n;i++){
      	if(p[i]==i) cnt++;
    }
  	return cnt==1;
}
```

```c++
//dfs判断，如果一个点一直dfs可以遍历所有点，则是图
class Solution {
public:
    vector<vector<int>> edge;
    void dfs(int i,vector<vector<int>>& edge,vector<int>& vis){
        if(vis[i]) return;
        vis[i]=1;
        for(auto& e:edge[i]){
            dfs(e,edge,vis);
        }
    }
    //经典dfs，如果可以一次遍历，就有一个连通块，则是图
    bool validTree(int n, vector<vector<int>>& edges) {
        if(edges.size()!=n-1) return false;
        edge.resize(n);
        for(auto& e:edges){
            edge[e[0]].push_back(e[1]);
            edge[e[1]].push_back(e[0]);
        }
        vector<int> vis(n,0);
        dfs(0,edge,vis);
        for(int i=0;i<n;i++){
            if(!vis[i]) return false;
        }
        return true;
    }
};
```

```c++
//bfs
class Solution {
public:
    vector<vector<int>> edge;
    bool validTree(int n, vector<vector<int>>& edges) {
        if(n-1!=edges.size()) return false;
        edge.resize(n);
        for(auto& e:edges){
            edge[e[0]].push_back(e[1]);
            edge[e[1]].push_back(e[0]);
        }
        queue<int> q;
        vector<int> vis(n,0);
        q.push(0);
        vis[0]=1;
        while(!q.empty()){
            auto node=q.front();
            q.pop();
            for(auto& e:edge[node]){
                if(!vis[e]){
                    q.push(e);
                    vis[e]=1;
                }
            }
        }
        for(int i=0;i<n;i++){
            if(!vis[i]) return false;
        }
        return true;
    }
};
```



# 冗余连接

```c++
//使用并查集判断
//无向图
const int N=1010;
int p[N];
int find(int x){	return p[x]==x? x: p[x]=find(p[x]);}
vector<int> findRedundantConnection(vector<vector<int>>& edges){
  	int n=edges.size();
  	for(int i=1;i<=n;i++) p[i]=i;
  	for(auto& e:edges){
      	int a=e[0],b=e[1];
      	if(find(a)==find(b)) return {a,b};
      	else p[p[a]]=p[b];
    }
}



//有向图
有向图由于连边具有方向，所以更加复杂，首先，如果加在根节点上，则所有节点入度都是1，存在环，如果不是根节点，那么存在一个节点入度为2，发生冲突。
因此有三种情况：
  1.有环无冲突
  2.有环有冲突，这种情况要返回冲突边的上一个父节点与该点连线
  3.无环有冲突
const int N=1010;
//一个存储父节点，一个存储祖宗节点，初始都是自身
int ancestor[N],p[N];
int find(int x){return ancestor[x]==x?x:ancestor[x]=find(ancestor[x]);}
vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
  	int n=edges.size();
  	for(int i=0;i<=n;i++){
      	p[i]=i;
      	ancestor[i]=i;
    }
  	int conflict=-1,cycle=-1;
  	for(int i=0;i<n;i++){
      	int a=edges[i][0],b=edges[i][1];
      	//b已经存在父节点
      	if(p[b]!=b) conflict= i;
      	else{
          	p[b]=a;
          	if(find(b)==find(a)) cycle=i;
          	else ancestor[find(b)]=find(a);
        }
    }
  	if(conflict<0){
      	auto ans=vector<int> {edges[cycle][0],edges[cycle][1]};
      	return ans;
    }
  	else{
      	if(cycle>0){
          	auto c=edges[conflict];
          	auto ans=vector<int> {p[c[1]],c[1]};
          	return ans;
        }
      	else {
          	auto ans=vector<int> {edges[conflict][0],edges[conflict][1]};
          	return ans;
        }
    }
}
```



# 树/图的存储方式

树是一种特殊的图，无环连通图

图：有向图、无向图，无向图是特殊的有向图。

## 1.邻接矩阵

```c++
g[N][N]
g[a][b]存储a-->b,有重边保留一个
比较浪费空间
```

## 2.邻接表

```c++
n个node开n个单链表，相当于拉链法hash的存储方式
int h[N],e[N],ne[N],idx;
void init(){
  idx=0;
  memset(h,-1,sizeof(h));
}
//在a开头的单链表中插入b
//头插法
void add(int a,int b){
  e[idx]=b;
  ne[idx]=h[a];
  h[a]=idx++;
}
```



# 拓扑排序

有向无环图->拓扑图

1.统计入度出度

2.入度为0的点作为起点

3.每次去掉入度为0的点以及边，不断迭代，直到去掉所有点

```c++
#include<iostream>
#include<queue>
#include<vector>
using namespace std;
int n,m;
vector<int> ans;
vector<vector<int>> outdegree(100010,vector<int>());
vector<int> indegree(100010,0);
int main(){
    cin>>n>>m;
    outdegree.resize(n);
    indegree.resize(n);
    for(int i=0;i<m;i++){
        int a,b;
        cin>>a>>b;
        indegree[b]++;
        outdegree[a].push_back(b);
    }
    queue<int> q;
    for(int i=1;i<=n;i++){
        if(indegree[i]==0) q.push(i);
    }
    while(!q.empty()){
        int node=q.front();
        ans.push_back(node);
        q.pop();
        for(auto& v:outdegree[node]){
            indegree[v]--;
            if(indegree[v]==0) q.push(v);
        }
    }
    if(ans.size()==n){
        for(int i=0;i<(int)ans.size();i++) cout<<ans[i]<<" ";
    }
    else cout<<-1;
    return 0;
}
```



拓扑排序也是可以找出有环图中所有在环里的点，就是在建图时，建立反图

```c++
//建立原图
outdegree[a].push_back(b);
//建立反图
outdegree[b].push_back(a);
```



# Dijkstra

## 朴素Dijkstra

```c++
//采用邻接矩阵储存
int g[N][N],dist[N];
bool st[N];
int Dijkstra(){
    memset(dist,0x3f,sizeof(dist));
    memset(st,false,sizeof(st));
    dist[1]=0;
    for(int i=0;i<n;i++){
        int t=-1;
        for(int j=1;j<=n;j++){
            if(!st[j]&&(t==-1||dist[t]>dist[j])){
                t=j;
            }
        }
        st[t]=true;
        for(int j=1;j<=n;j++){
            dist[j]=min(dist[j],dist[t]+g[t][j]);
        }
    }
    return dist[n]==0x3f3f3f3f? -1:dist[n];
}
```

## 堆优化Dijkstra

```c++
//用邻接表储存
```

# 最小生成树

## prim算法

思想类似于Dijkstra，从一个空集开始，每次加入结点，然后更新距离，每次都要加入两个集合V S-V的最小权重的边连接的结点。

1.初始化dist，st，g数组，dist数组无穷大，表示各个点到连通部分的距离，不断更新，st代表结点是否已经被选择过

2.选中权重最小的结点，更新距离，加入V，直到所有结点都加入

```c++
#include<iostream>
#include<cstring>
using namespace std;
const int N=510;
int g[N][N],dist[N],n,m;
bool st[N];
int prim(){
	memset(dist,0x3f,sizeof(dist));
	int res=0;
	for(int i=0;i<n;i++){
  		int t=-1;
  		for(int j=1;j<=n;j++){
    	  	if(!st[j]&&(t==-1||dist[t]>dist[j])){
          		t=j;
       		}
    	}
  		if(i&&dist[t]==0x3f3f3f3f) return 0x3f3f3f3f;
  		if(i) res+=dist[t];
  		st[t]=true;
  		for(int j=1;j<=n;j++){
      		dist[j]=min(dist[j],g[t][j]);
    	}
	}
    return res;
}
int main(){
    cin>>n>>m;
    int u,v,w;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            if(i==j) g[i][j]=0;
            else g[i][j]=0x3f3f3f3f;
        }
    }
    while(m--){
        cin>>u>>v>>w;
        g[u][v]=g[v][u]=min(g[v][u],w);
    }
    int ans=prim();
    if(ans==0x3f3f3f3f) cout<<"impossible"<<endl;
    else cout<<ans;
    return 0;
}
```

![img](https://fc.dianhsu.top/lc?user=cloneyate&loc=cn&req=rating)[]()
