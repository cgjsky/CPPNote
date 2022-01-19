# Trie树

高效的存储与查找字符串集合的数据结构

```c++
#include<iostream>
using namespace std;
const int N=100010;
//son[N][26]--每个结点最多有26个子结点
//cnt[N]以该结点结束的字符串的个数
//idx总共的结点数，当插入结点时，不存在就插入第idx个结点填补
int son[N][26],cnt[N],idx;
//一定要初始化，一定要初始化，一定要初始化
void init(){
  memset(cnt,0,sizeof cnt);
  memset(son,0,sizeof son);
  idx = 0;
}
void insert(string a){
    int p=0;//从根结点开始访问
    for(auto ch:a){
        int u=ch-'a';
        if(!son[p][u]) son[p][u]=++idx;
        p=son[p][u];
    }
    cnt[p]++;
}
int search(string a){
    int p=0;
    for(auto ch:a){
        int u=ch-'a';
        if(!son[p][u]) return 0;
        p=son[p][u];
    }
    return cnt[p];
}
int main(){
    int n;
    cin>>n;
    while(n--){
        string op,str;
        cin>>op>>str;
        if(op[0]=='I') insert(str);
        else cout<<search(str)<<endl;
    }
    return 0;
}
```

# 并查集

1.将两个集合合并

2.询问两个元素是否在一个集合当中

基本原理：每个集合用一个树表示，树根的编号就是整个集合的编号

每个结点存储它的父结点，p[x]表示x的父结点

```c++
//问题1:如果判断树根
if(p[x]==x)
//问题2:如何求x的集合编号
while(p[x]!=x) x=p[x]
//问题3:如何合并
//假设px是x的集合编号，py是y的集合编号
p[x]=y

//路径压缩优化
当对于一个结点x，一步步访问p[x]到根结点后，使得其路径上所有结点的p[x]都指向根结点
增加路径压缩后，可以从O(long n)提高到O(1)
```

```c++
//朴素并查集
#include<iostream>
using namespace std;
const int N=100010;
int p[N],n;
void init(){
    cin>>n;
    for(int i=0;i<n;i++) p[i]=i;
}
//返回祖宗结点+路径压缩
int find(int x){
    if(p[x]!=x) p[x]=find(p[x]);
    return p[x];
}
int main(){
    init();
    int m;
    cin>>m;
    while(m--){
        string op;
        int a,b;
        cin>>op>>a>>b;
        if(op[0]=='M') p[find(a)]=find(b);
        else if(find(a)==find(b)) cout<<"Yes"<<endl;
        else cout<<"No"<<endl;
    }
    return 0;
}
```


