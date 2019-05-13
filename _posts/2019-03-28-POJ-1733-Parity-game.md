---
layout: post
title:  "我曾以为我会并查集"
date:   2019-03-06
desc: "POJ - 1733 Parity game"
keywords: "POJ"
categories: [Acm]
tags: [并查集,边带权,扩展域]
icon: icon-html
---

# POJ - 1733 Parity game

>我也不知道我咕了多久，这个月发生了好多事情，首先是庄学姐甩锅，我们要自立更生了，然后最近蓝桥杯自闭，今天又气走了大奶牛。。。18级前途渺茫啊。。。
>
>说一下这个题吧。。我曾经以为我会并查集。。但是我今天发现我错了。。。

这个题蓝书上有两种做法，代码基本看懂后照着打

用 **sum[ x ]** 表示 **1 ~ x** 中1的个数

那么，在 **l ~ r** 中如果1的个数为奇数，则说明 **sum[ l - 1 ] ,  sum[ r ]** 奇偶性不同，反之则说明相同。

于是用0表示两个点的奇偶性相同，1表示奇偶性不同

因为读入的 **l , r** 的范围为1e9，但是对问题产生影响的只有其中的1的个数的奇偶性，所以 **l , r** 之间的数字并没有用处，于是要先进行离散化

## 边带权

前面做了一道边带权的并查集，所以这个方法理解起来还算是比较容易

所谓边带权就是把一个并查集当作一颗树，然后每条边上都有一个权值，如果不用并查集的话，直接在树上搜索会用很长时间，所以并查集里的路径压缩是必要的

但是路径压缩后，原本的边就变成了一个直接指向根节点的边，所以新边的边权应该等于原先该点到根节点的路径上边权的和，用一个d数组记录到根节点的距离，边权级可以随着路径一起压缩

设一条边的边权为0代表两个点的奇偶性相同，为1代表奇偶性不同，则路径压缩时只要对边权进行异或运算，就可以将边权同时压缩

对两个点 **x = l - 1 , y = r** ，令 **p = get (x) , q = get (y)** ，**ans** 表示该问题的回答（由 0，1 来表示）

如果p=q，则两个点在同一个并查集中，如果连个点之间的奇偶性关系与ans不同，则小A在撒谎，否则就跳过

如果p!=q，则两个点不再同一个并查集中，所以需要把他们连起来，先使 ** fa[p]=q** ，对于边权来说，x与y之间的路径由 **x ~ p , p ~ q , q ~ y** 三部分组成，所以 **ans=d[x]\^d[y]\^d[p]** ，**d[p]=d[x]\^d[y]\^ans**。

```c++
#include<iostream>
#include<iomanip>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
#include<queue>
#include<stack>
#include<vector>
#include<string>
#include<map>
#include<set>
#include<ctime>
#define ll  long long
#define ull unsigned long long
using namespace std;
const int N=2e4+5,M=10007;
const ull base=13331;
const double Pi=acos(-1.0);
const ll C=299792458;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
int n,m;
struct node{
    int l,r,ans;
}query[N];
int a[N],fa[N],d[N];
void read_discrete(){
    cin>>n>>m;
    int t=0;
    for(int i=1;i<=m;i++){
        cin>>query[i].l>>query[i].r;
        string s;
        cin>>s;
        query[i].ans=((s[0]=='o')?1:0);
        a[++t]=query[i].l;
        a[++t]=query[i].r;
    }
    sort(a+1,a+t+1);
    n=unique(a+1,a+t+1)-a-1;
}
int get(int x){
    if(x==fa[x])return x;
    int root=get(fa[x]);
    d[x]^=d[fa[x]];
    return fa[x]=root;
}
int main()
{
    ios::sync_with_stdio(false);
    read_discrete();
    for(int i=1;i<=n;i++)fa[i]=i;
    for(int i=1;i<=m;i++)
    {
        int x=lower_bound(a+1,a+n+1,query[i].l-1)-a;
        int y=lower_bound(a+1,a+n+1,query[i].r)-a;
        int p=get(x),q=get(y);
        if(p==q){
            if((d[x]^d[y])==query[i].ans)continue;
            cout<<i-1<<endl;
            return 0;
        }
        fa[p]=q;
        d[p]=d[x]^d[y]^query[i].ans;
    }
    cout<<m<<endl;
}
```

## 扩展域

其实这个题还是用扩展域比较好理解一些

对于两个点 **x , y** ，他们之间是无法同时表示奇偶性相同与奇偶性不同的，所以可以把一个点拆分成 **x_odd** 与 **x_even** 两个点，那么两个点奇偶性相同就可以表示成 **get(x_odd)==get(y_odd) , get(x_even)==get(y_even)** ，奇偶性不同就是 **get(x_odd)==get(y_even) , get(x_even)==get(y_odd)** 

所以查询一个询问，如果与之前的奇偶性关系矛盾，那么这个回答就是错误的

```c++
#include<iostream>
#include<iomanip>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
#include<queue>
#include<stack>
#include<vector>
#include<string>
#include<map>
#include<set>
#include<ctime>
#define ll  long long
#define ull unsigned long long
using namespace std;
const int N=2e4+5,M=10007;
const ull base=13331;
const double Pi=acos(-1.0);
const ll C=299792458;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
int n,m;
struct node{
    int l,r,ans;
}query[N];
int fa[N*2];
int a[N];
void read_discrete(){
    cin>>n>>m;
    int t=0;
    for(int i=1;i<=m;i++){
        cin>>query[i].l>>query[i].r;
        string s;
        cin>>s;
        query[i].ans=((s[0]=='o')?1:0);
        a[++t]=query[i].l;
        a[++t]=query[i].r;
    }
    sort(a+1,a+t+1);
    n=unique(a+1,a+t+1)-a-1;
}
int get(int x){
    if(x==fa[x])return x;
    return fa[x]=get(fa[x]);
}
int main()
{
    ios::sync_with_stdio(false);
    read_discrete();
    for(int i=1;i<=2*n;i++)fa[i]=i;
    for(int i=1;i<=m;i++)
    {
        int x=lower_bound(a+1,a+n+1,query[i].l-1)-a;
        int y=lower_bound(a+1,a+n+1,query[i].r)-a;
        int x_odd=x,x_even=x+n;
        int y_odd=y,y_even=y+n;
        if(query[i].ans==0){
            if(get(x_odd)==get(y_even)){
                cout<<i-1<<endl;
                return 0;
            }
            fa[get(x_odd)]=get(y_odd);
            fa[get(x_even)]=get(y_even);
        }
        else{
            if(get(x_even)==get(y_even)){
                cout<<i-1<<endl;
                return 0;
            }
            fa[get(x_odd)]=get(y_even);
            fa[get(x_even)]=get(y_odd);
        }
    }
    cout<<m<<endl;
}
```
