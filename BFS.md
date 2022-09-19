# 迷宫问题

一定要注意迷宫问题的二维矩阵不一定长宽都是n，可能一个n，一个m，不要再WA了
a
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

****

BFS剪枝问题

[单词接龙](https://leetcode-cn.com/problems/word-ladder/)

这道题，一眼BFS，但是第一次做的时候，TLE了，因为在BFS选择下一个元素入队的时候，用的是与wordlist的每个单词对比，如果有一个字母不一致且没有访问过，入队，但是这样会导致每次都O（n）的复杂度。

优化思路：不用每次对比，而是将单词的所有变化，全部找出来，然后对于set化的wordlist，进行对比，如果没有访问过，并且在set里面，就入队，这样的话，当单词的长度小的时候，消耗的时间少，题目给的是`1 <= beginWord.length <= 10`

所以，我们选择第二种

BFS判断一般两种：

1⃣️：直接拿字典遍历，寻找合适的  O(n)

2⃣️：找出所有变化，然后判断是否在字典里，这样的话只用O(1)