# 单链表

最常用的是邻接表，存储图和树

双链表-优化某些问题

```c++
//静态单链表
//使用e[N],ne[N]分别存储val，next
#include<iostream>
using namespace std;
const int N=100010;
int e[N],ne[N];
//head初始指向-1，之后不断更新，一直在最前面，而-1一直是末尾，相当于NULL
int head,idx;
//初始化
void init(){
    head=-1;
    idx=0;
}
//在头结点前加入结点
void add_to_head(int x){
    e[idx]=x;
    ne[idx]=head;
    head=idx++;
}
//表示在第k个插入的数后面插入一个数 x
void add(int k,int x){
    e[idx]=x;
    ne[idx]=ne[k];
    ne[k]=idx;
    idx++;
}
//表示删除第k个插入的数后面的数（当k为0时，表示删除头结点）
void del(int k){
    ne[k]=ne[ne[k]];
}
int main(){
    int M;
    init();
    cin>>M;
    while(M--){
        int k,x;
        char op;
        cin>>op;
        if(op=='H') 
        { 
            cin>>x;
            add_to_head(x);
        }
        if(op=='D') {
            cin>>k;
            if(!k) head=ne[head];
            else del(k-1);
        }
        if(op=='I') {
            cin>>k>>x;
            add(k-1,x);
        }
    }
    for(int i=head;i!=-1;i=ne[i]) cout<<e[i]<<" ";
    return 0;
}
```

# 双链表

```c++
#include<iostream>
using namespace std;
const int N=100010;
int e[N],l[N],r[N];
int idx;
//初始化
void init(){
    r[0]=1;
    l[1]=0;
  //idx从2开始
    idx=2;
}
////在第k个节点后插入x
void insert(int k,int x){
    e[idx]=x;
    l[idx]=k;
    r[idx]=r[k];
    l[r[k]]=idx;
    r[k]=idx;
    idx++;
}
//删除第k个节点
void del(int k){
    r[l[k]]=r[k];
    l[r[k]]=l[k];
}
int main(){
    init();
    int M,k,x;
    string opt;
    cin>>M;
    while(M--){
        cin>>opt;
        if(opt=="L"){
            cin>>x;
            insert(0,x);
        }
        if(opt=="R"){
            cin>>x;
            insert(l[1],x);
        }
        if(opt=="D"){
            cin>>k;
            del(k+1);
        }
        if(opt=="IL"){
            cin>>k>>x;
            insert(l[k+1],x);
        }
        if(opt=="IR"){
            cin>>k>>x;
            insert(k+1,x);
        }
    }
  //从r[0]开始读
    for(int i=r[0];i!=1;i=r[i]) cout<<e[i]<<" ";
    return 0;
}
```

# 栈/队列

数组模拟

```c++
// tt表示栈顶
int stk[N], tt = 0;
// 向栈顶插入一个数
stk[ ++ tt] = x;
// 从栈顶弹出一个数
tt -- ;
// 栈顶的值
stk[tt];
// 判断栈是否为空
if (tt > 0)
{
  return false;
}

// hh 表示队头，tt表示队尾
int q[N], hh = 0, tt = -1;
// 向队尾插入一个数
q[ ++ tt] = x;
// 从队头弹出一个数
hh ++ ;
// 队头的值
q[hh];
// 判断队列是否为空
if (hh <= tt)
```



单调栈/单调队列 保证一个单增或单减的性质

```c++
//单调栈
#include<iostream>
#include<vector>
#include<stack>
using namespace std;
const int N=100010;
int a[N];
stack<int> sk;int n;
int main(){
    cin>>n;
    for(int i=0;i<n;i++) cin>>a[i];
    sk.push(a[0]);
    cout<<"-1"<<" ";
    for(int i=1;i<n;i++){
        while(!sk.empty()&&sk.top()>=a[i]) sk.pop();
        if(sk.empty()) cout<<"-1"<<" ";
        else cout<<sk.top()<<" ";
        sk.push(a[i]);
    }
    return 0;
}
```



```c++
//单调队列 求滑动窗口中的最大最小值
#include<iostream>
using namespace std;
const int N=1000010;
int a[N],q[N];
int n,k;
int main(){
    cin.tie(0);
    cin>>n>>k;
    for(int i=0;i<n;i++) cin>>a[i];
    int hh=0,tt=-1;
    for(int i=0;i<n;i++){
        //hh弹出
        if(hh<=tt&&i-k+1>q[hh]) ++hh;
        //tt弹出
        while(hh<=tt&&a[q[tt]]>=a[i]) tt--;
        //新值进入
        q[++tt]=i;
        //输出
        if(i>=k-1) cout<<a[q[hh]]<<" ";
    }
    hh=0,tt=-1;
    cout<<endl;
    for(int i=0;i<n;i++){
        if(hh<=tt&&i-k+1>q[hh]) ++hh;
        while(hh<=tt&&a[q[tt]]<=a[i]) tt--;
        q[++tt]=i;
        if(i>=k-1) cout<<a[q[hh]]<<" ";
    }
    return 0;
}
```



# **KMP**

下标从1开始

```c++
#include<iostream>
using namespace std;
const int N=1000010;
char s[N],p[N];
int ne[N];
int m,n;

int main(){
    //注意p，s数组都是从1开始的；
    cin>>n>>p+1>>m>>s+1;
    //next数组,r=2是因为ne从1开始，且ne[1]=0；
    for(int l=0,r=2;r<=n;r++){
        while(l&&p[r]!=p[l+1]) l=ne[l];//当不匹配时，l退位，切记不可一次退到0；
        if(p[r]==p[l+1]) l++;//匹配时，l进位
        ne[r]=l;//1-r字符串的最大前缀是l
    }
    //kmp匹配
    for(int i=1,j=0;i<=m;i++){
        while(j&&s[i]!=p[j+1]) j=ne[j];//不匹配，l退位
        if(s[i]==p[j+1]) j++;
        if(j==n){
            cout<<i-n<<" ";
            j=ne[j];
        }
    }
    return 0;
}
```

下标从0开始，基本上是上面的L-1

```c++
const int N=100010;
class Solution {
int ne[N];
public:
    int strStr(string haystack, string needle) {
        if(needle=="") return 0;
        for(int r=1,l=0;r<needle.size();r++){
            while(l&&needle[r]!=needle[l]) l=ne[l-1];
            if(needle[l]==needle[r]) l++;
            ne[r]=l;
        }
        for(int i=0,j=0;i<haystack.size();i++){
            while(j&&haystack[i]!=needle[j]) j=ne[j-1];
            if(haystack[i]==needle[j]) j++;
            if(j==needle.size()){
                return i-needle.size()+1;
                j=ne[j];//如果有继续匹配的需求
            }
        }
        return -1;
    } 
};
```

