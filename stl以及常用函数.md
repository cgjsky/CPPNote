# 输入输出

```c++
//cin.tie(0);
//输出建议不要cout<<endl;而是直接在原cout里加"\n";
```

# 取整

```c++
//向上取整：
（x+k-1）/k
//向下取整
x/k
```



# memset

```c++
//只能memset一些特殊值
memset(a,0,sizeof a);
memset(a,0x3f,sizeof a);
memset(a,-1,sizeof a);
memset 1会变成16843009
```



# 万能头文件

```c++
#include<bits/stdc++.h>
```



# 强制性类型转化

```c++
//long long转int
typedef long long ll;
int x;
ll a=static_cast<ll>(x)
  

//strin转int long long long 
stoi stol stoll
string str1="45";string str2="2 words and "
string str3="2www3"
int x1=stoi(str1); //x1=45
int x2=stoi(str2); //x2错误
int x3=stoi(str3); //x2=2
stoi\stol\stoll使用时，将字符串转化为数字类型，但是必须要以数字开头，且只转化到字母前的数字
  
//int long ll转string
int x=45;
string s=to_string(x);//s="45"
```

# 查看数据类型

```
cout<<typeid(x).name();
有些编译器只返回类型首字母； int->i string->Ss
```

# 判断字母/数字

```c++
isalnum(c)　　// 当为字母或数字时为真
isalpha(c)　　// 当为字母时为真
isdigit(c)　　// 当为数字时真
islower(c)　　// 当为小写字母时为真
issupper(c)　　// 当为大写字母时为真
isspace(c)　　// 当为空格时为真
tolower(c)　　// 转换为小写字母， 当本身为小写字母时，原样输出
toupper(c)　　// 转换为大写字母， 当本身为大写字母时，原样输出

string vowels = "aeiouAEIOU";
vowels.find(x)!=-1 //判断是否是原音

s.insert(s.size(), i + 1, 'a')//在s后面加i+1个a
```

```c++
//反转字符串
string x;
string x_rev(x.rbegin(), x.rend());

//处理字符串，保留数字，字母
string x;
for (char ch: s) {
    if (isalnum(ch)) {
        x += tolower(ch);
    }
}
```



# STL

```c++
vector, 动态数组，倍增的思想
//初始化：
vector<int> a(n,x) 长度为10，每一个初始化为x；
    size()  返回元素个数 //O(1)
    empty()  返回是否为空 //O(1)
    clear()  清空,改变size，数组capacity保持不变
    front() /back() 返回头元素，尾元素
    push_back()/pop_back()
    begin()/end()
    []
    支持比较运算，按字典序

pair<int, int>//自带结构体，支持比较
    first, 第一个元素
    second, 第二个元素
    支持比较运算，以first为第一关键字，以second为第二关键字（字典序）

string，字符串
    size()/length()  返回字符串长度
    empty()
    clear()
    pop_back() 删除最后一个字符
    substr(起始下标，(子串长度))  返回子串
    c_str()  返回字符串所在字符数组的起始地址
    find(ch,pos = 0) 从字符串的pos位置开始，查找字符ch(也可以查找字符串)。如果找到，则返回该子字符串首次出现的位置；否则，返回string::npos
    //int idx=a.find(b);
    //if(idx!=string::npos){...}

queue, 队列
    size()
    empty()
    push()  向队尾插入一个元素
    front()  返回队头元素
    back()  返回队尾元素
    pop()  弹出队头元素

priority_queue, 优先队列，默认是大根堆
    size()
    empty()
    push()  插入一个元素
    top()  返回堆顶元素
    pop()  弹出堆顶元素
    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;

stack, 栈
    size()
    empty()
    push()  向栈顶插入一个元素
    top()  返回栈顶元素
    pop()  弹出栈顶元素

deque, 双端队列//效率低，功能强
    size()
    empty()
    clear()
    front()/back()
    push_back()/pop_back()
    push_front()/pop_front()
    begin()/end()
    []

set, map, multiset, multimap, 基于平衡二叉树（红黑树），动态维护有序序列
//set、map不允许重复元素，multiset、multimap可以，都是有序的
    size()
    empty()
    clear()
    begin()/end()
    ++, -- 返回前驱和后继，时间复杂度 O(logn)

    set/multiset
        insert()  插入一个数
        find()  查找一个数
        count()  返回某一个数的个数
        //set的count只返回0，1，multi返回真实个数
        erase()
            (1) 输入是一个数x，删除所有x   O(k + logn)
            (2) 输入一个迭代器，删除这个迭代器
        lower_bound()/upper_bound()
        lower_bound(x)  返回大于等于x的最小的数的迭代器
        upper_bound(x)  返回大于x的最小的数的迭代器
    map/multimap//映射
        insert()  插入的数是一个pair
        erase()  输入的参数是pair或者迭代器
        find()
        []  可以像数组一样使用
        注意multimap不支持此操作。 时间复杂度是 O(logn)
        lower_bound()/upper_bound()

unordered_set, unordered_map, unordered_multiset, unordered_multimap, 哈希表
    和上面类似，增删改查的时间复杂度是 O(1)
    不支持 lower_bound()/upper_bound()， 迭代器的++，--

bitset, 圧位
    bitset<10000> s;<>里面是数组长度，不是类型
    ~, &, |, ^
    >>, <<
    ==, !=
    []

    count()  返回有多少个1

    any()  判断是否至少有一个1
    none()  判断是否全为0

    set()  把所有位置成1
    set(k, v)  将第k位变成v
    reset()  把所有位变成0
    flip()  等价于~
    flip(k) 把第k位取反
```

# 字符串提取单词

```c++
//对于字符串，带有空格，提取字符串中的每个单词到string数组中，之后再进行处理
s+=" ";//首先加上" "来对最后一个字母进行特殊处理
vector<string> ans;
string tmp="";
for(auto& ch:s){
  if(ch==' '){
    if(!tmp.empty()){
      ans.push_back(tmp);
      tmp.clear();
    }
  }
  else tmp+=ch;
}
```

