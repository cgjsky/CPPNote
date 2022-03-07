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

