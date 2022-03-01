# 树/图的存储方式

## 1.

## 2.

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

