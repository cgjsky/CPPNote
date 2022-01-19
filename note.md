# 队列与优先队列

## 队列：前出后进

```c++
queue <int> q;
q.push(x);
q.pop();
q.size();
q.empty();
q.front();//返回第一个元素
q.back;//返回最后一个元素
```



## 优先队列

```c++
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> q;
```

队列里的元素是pair,通过pair.first pair.second来访问pair里的内容

通过make_pair()来让两个int合并成pair并push进队列

## 用两个栈来模拟队列

栈FILO 队列FIFO 所以需要两个栈，一个用来push，一个用来pop以及peek

```c++
stack<int> instack,outstack;
void in2out(){
    while(!instack.empty()){
       outstack.push(instack.top());
       instack.pop()
    }
}
void push(int x) {
       instack.push(x);
    }
int pop() {
       if(outstack.empty()){
            in2out();
        }
        int x=outstack.top();
        outstack.pop();
        return x;
    }
int peek() {
        if(outstack.empty()){
            in2out();
        }
        int x=outstack.top();
        return x;
}
bool empty() {
        return instack.empty()&&outstack.empty();
}
```



# 无重复最长子串（滑动窗口）

建立一个hash集合解决重复问题

不断滑动窗口，寻找最大的子串

```C++
unordered_set<char> occ;
int ans=0;
int rk=0;
for(int i=0;i<s.size();i++){
    //首先排除当i=0时的数组冲突问题
    while(i!=0){
       occ.earse(s[i-1]);
    }
    while(rk<s.size()&&!occ.count(s[rk])){
        occ.insert(s[rk]);
        rk++;
    }
    ans=max(ans,rk-i);
}
```

# Unordered_map与Unorder_set

unordered_map是key->value的映射

```c++
#include<unordered_map>
unordered_map<string,int> ans;
ans["ABC"]=1;
ans["A"]=2;
for(auto& item:ans){
    cout<<item.first<<' '<<item.second<<endl;
}
```

unordered_set实现了不存储重复的元素。

unordered_map实现了key和value的映射

```c++
unordered_map<int,int> ans;
for(char ch:s){
  ++ans[ch];
}
```

# 动态规划

一般情况下，找出状态转移方程，然后return dp[n-1]；

根据题目的特殊情况，具体分析，比如可能建立两个方程去维持状态，或者返回数组的最大值

```c++
return *max_element(ans.begin(),ans.end());
```

特殊例子：**LC1567**

# BFS

广度优先搜素一般需要queue去实现。

1.把初始结点加入队列，开始搜索。

2.保存当前结点的信息，并将其排除队列。

3.进行判断在当前结点信息下，要加入队列的结点。

4.循环以上步骤

```c++
queue<pair<int,int>> q;
q.emplace(a,b);
while(!q.empty){
   //保存信息
   int x=q.front().first;
   int y=q.front().second;
   //排除结点
   q.pop();
  //判断信息，加入新结点。
  if(....){
    q.emplace(...);
  }
}
```

```c++
queue<TreeNode*> q;
q.push(root);//根结点进入
int level=0;
while(!q.empty()){
   int size=q.size();
   auto node=q.front();
   int vlaue=node->value;
   for(int i=0;i<size;i++){
       .....
   }
   level++;
}
```

有时候，确保我们每个结点只访问一次很重要，因此我们建立哈希集去解决这个问题

```c++
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> used;     // store all the used nodes
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to used;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to used;
                }
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```

# DFS

**DFS与递归密不可分**

大部分情况，我们在可以使用BFS的时候也可以使用DFS，但是要注意最短路径问题，因为DFS最早访问到的目标结点的路径不一定是最短的，

```c++
/*
 * Return true if there is a path from cur to target.
 */
boolean DFS(Node cur, Node target, Set<Node> visited) {
    return true if cur is target;
    for (next : each neighbor of cur) {
        if (next is not in visited) {
            add next to visted;
            return true if DFS(next, target, visited) == true;
        }
    }
    return false;
}
```

我们上面用的是系统的隐式栈，当数据过多时，会产生栈崩溃，这是我们用显式栈来解决。

```c++
/*
 * Return true if there is a path from cur to target.
 */
boolean DFS(int root, int target) {
    Set<Node> visited;
    Stack<Node> s;
    add root to s;
    while (s is not empty) {
        Node cur = the top element in s;
        return true if cur is target;
        for (Node next : the neighbors of cur) {
            if (next is not in visited) {
                add next to s;
                add next to visited;
            }
        }
        remove cur from s;
    }
    return false;
}
```

# 链表

## 反转链表

### 迭代法

```c++
ListNode* reverseList(ListNode* head) {
  ListNode*p=nullptr;
  ListNode*q=head;
  while(!q){
    ListNode*next=q->next;
    q->next=p;
    p=q;
    q=next;
  }
}
```

### 递归

```c++
ListNode* reverseList(ListNode* head) {
  if(!head||!head->next) return head;
  ListNode*newhead=reverselList(head->next);
  head->next->next=head;
  head->next=nullptr;
  return newhead;
}
```

# 二叉树

## 遍历

### 中序遍历

#### 递归

```c++
void inorder(TreeNode*root,vector<int>& ans){
        if(!root) return;
        inorder(root->left,ans);
        ans.push_back(root->val);
        inorder(root->right,ans);
    }
vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        inorder(root,ans);
        return ans;
    }
```

```
int dirs[4][2]={{1,0},{-1,0},{0,1},{0,-1}};
    int orangesRotting(vector<vector<int>>& grid) {
        queue<pair<int,int>> q;
        int m=grid.size();
        int n=grid[0].size();
        int count=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==2){
                    q.emplace(i,j);
                }
            }
        }
        if(q.empty()) count=1;
        while(!q.empty()){
            for(int k=0;k<q.size();k++){
                 auto [x,y]=q.front();
                 q.pop();
                 for(int i=0;i<4;i++){
                    int xx=x+dirs[i][0];
                    int yy=y+dirs[i][1];
                    if(xx>=0&&xx<m&&yy>=0&&yy<n&&grid[xx][yy]==1){
                      grid[xx][yy]=2;
                      q.emplace(xx,yy);
                }
            }
          }
           count++;
        }
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1){
                    return -1;
                }
            }
        }
        return count-1;
    }
```

# 数组

## 一维数组转化为二维数组

```c++
vector<vector<int>> construct2DArray(vector<int>& original, int m, int n) {
	if(original.size()!=m*n) return {};
	vector<vector<int>> ans;
	for(auto item=original.begin();item!=original.end();item+=n){
		ans.emplace_back(item,item+n);
	}
}
```



