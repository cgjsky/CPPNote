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
p[px]=py

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
//理解为，p[x]是x的父结点，当一个结点父结点不是自己时，证明不是根结点，此时，它的父结点是find(p[x])，find函数实现路径压缩，把整个路径上的所有结点都连接到根结点
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

## 拓展并查集

增加size数组来保存连通集团内的结点数量，当进行连接操作时，先把size加过去，再去进行祖宗结点的修改

```c++
void init(){
    cin>>n;
    for(int i=1;i<=n;i++){
        p[i]=i;
        s[i]=1;
    }
}
//一定要先加size再连接，否则连接之后size会改变
s[find(b)]+=s[find(a)];
p[find(a)]=find(b);
```

用并查集维护到祖宗结点的距离

```c++
#include<iostream>
using namespace std;
const int N=100010;
//d[N]维护到祖宗结点距离
int p[N],d[N],n,k;
int ans=0;
void init(){
    cin>>n;
    for(int i=1;i<=n;i++){
        p[i]=i;
    }
}
int find(int x){
    if(p[x]!=x)
    {
        int t=p[x];
      //更新p[x],压缩路径
        p[x]=find(p[x]);
      //更新d[x]
        d[x]+=d[t];
    }
    return p[x];
}
int main(){
    init();
    cin>>k;
    while(k--)
    {
        int t,x,y;
        cin>>t>>x>>y;
        if(x>n || y>n ) ans++;
        else
        {
            int px=find(x),py=find(y);
            if(t==1)
            {
                if(px==py&&(d[x]-d[y])%3!=0) ans++;
                else if(px!=py)
                {
                    p[px]=p[y];
                    d[px]=d[y]-d[x];
                }
            }
            else
            {
                if(px==py&&(d[x]-d[y]-1)%3) ans++;
                else if(px!=py)
                {
                    p[px]=p[y];
                    d[px]=d[y]+1-d[x];
                }
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}
```



# 手写堆

堆是完全二叉树

```c++
//小根堆--根结点小于等于左右结点
//大根堆--根结点大于等于左右结点
用一维数组存储，x表示当前结点下标，
2x表示当前结点的左儿子
2x+1表示当前结点的右儿子
```



```c++
//下标从1开始
1.插入一个数     
head[++size]=x;
up(size);//size点上移操作

2.求集合中的最小值 
heap[1];

3.删除最小值
head[1]=head[size--];
down(1)//1号点下移
  
4.删除任意元素
head[k]=heap[size--];
down(k);up(k);//只会执行一个

4.修改任意元素
heap[k]=x;
down(k);up(k);
```

```c++
// h[N]存储堆中的值, h[1]是堆顶，x的左儿子是2x, 右儿子是2x + 1
// ph[k]存储第k个插入的点在堆中的位置
// hp[k]存储堆中下标是k的点是第几个插入的
int h[N], ph[N], hp[N], size;
// 交换两个点，及其映射关系
void head_swap(int a,int b){
    swap(ph[hp[a]],ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(h[a], h[b]);
}
void down(int u)
{
    int t = u;
    if (u * 2 <= size && h[u * 2] < h[t]) t = u * 2;
    if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
    if (u != t)
    {
        heap_swap(u, t);
        down(t);
    }
}
void up(int u)
{
    while (u / 2 && h[u] < h[u / 2])
    {
        heap_swap(u, u / 2);
        u >>= 1;
    }
}
// O(n)建堆
for (int i = n / 2; i; i -- ) down(i);
```

# Hash

hash与离散化的区别：

离散化是一种很特殊的hash方式，离散化是需要保序的，而hash是无序的，离散化需要用到相对位置，hash不需要，而且离散化不会带来冲突

## hash存储

1.开放寻址法
2.拉链法

```c++
//拉链法————开一个一维数组存储所有的hash值
//每个位置有一个拉链，存入x时，在数组位置拉一个链
#include<iostream>
#include<cstring>
using namespace std;
const int N=100003;//取质数
int h[N],e[N],ne[N];
int idx;
void init(){
    memset(h,-1,sizeof(h));
    idx=0;
}
//插入操作
//用单链表头插法解决冲突
void insert(int x){
    int k=(x%N+N)%N;
    e[idx]=x;
    ne[idx]=h[k];
    h[k]=idx++;
}
//查询操作
bool query(int x){
    int k=(x%N+N)%N;
    for(int i=h[k];i!=-1;i=ne[i]){
        if(e[i]==x) return true;
    }
    return false;
}
```

```c++
//开放寻址法--依次找位置
//N一般开到2-3倍
const int N=200003,null=0x3f3f3f3f
int h[N];//N一般开到2-3倍
memset(h,0x3f,sizeof(h));
// 如果x在哈希表中，返回x的下标；如果x不在哈希表中，返回x应该插入的位置
int find(int x)
{
  int t = (x % N + N) % N;
  while (h[t] != null && h[t] != x)
  {
     t ++ ;
     if (t == N) t = 0;
  }
 return t;
}
```



## 字符串hash

```c++
这里的哈希方式是一些比较特殊的哈希方式，即字符串前缀哈希。比如有一个字符串abcde，则可以先预处理出来所有前缀的哈希，比如h[1]就表示a的哈希，h[2]就表示ab的哈希，特别的，定义h[0]=0表示前0个字符的哈希值为0。
要定义某一个前缀的哈希值，只要把字符串看成是一个 P P P进制的数，那么每一位的ASCII码就表示这一位的数字是多少。那么上面的例子的字符串哈希值（即对应的十进制的数值）为：
```

$$
a*P^3+b*P^2+c^P+d\ \ \mod \,Q
$$

完全不考虑hash 冲突，P=131 Q=2^64
$$
Hash[L..R]=(Hash[R]−Hash[L−1]×P^{R−L+1}+2^{64})mod2^{64}
$$

```c++
#include<iostream>
using namespace std;
const int N=100010;
//ULL不用再对2的64次方取mod
typedef unsigned long long ULL;
ULL h[N],p[N];
//经典P值
const int P = 131;
ULL query(int l,int r){
    return h[r]-h[l-1]*p[r-l+1];
}
int main(){
    int n,m;
    cin>>n>>m;
    string s;
    cin>>s;
    p[0]=1;
    h[0]=0;
    for(int i=0;i<n;i++){
        p[i+1]=p[i]*P;
        h[i+1]=h[i]*P+s[i];
    }
    while(m--){
        int l1,l2,r1,r2;
        cin>>l1>>r1>>l2>>r2;
        if(query(l1,r1)==query(l2,r2)) cout<<"Yes"<<endl;
        else cout<<"No"<<endl;
    }
    return 0；   
}
```



