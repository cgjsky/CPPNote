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
void add(int a,int b){
  e[idx]=b;
  ne[idx]=h[a];
  h[a]=idx++;
}
```



# Dijkstra

## 朴素Dijkstra

```c++
//采用邻接表储存
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

