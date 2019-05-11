---
layout: post
title:  "2019 CCPC Wannafly Winter Camp Day7"
date:   2019-01-26
desc: "2019 CCPC Wannafly Winter Camp"
keywords: "Camp"
categories: [Acm]
tags: [2019 CCPC Wannafly Winter Camp]
icon: icon-html
---

_____

今天的博客更的有些晚，没有办法，晚上一场cf，白天又自闭，直接鸽掉了，嘤嘤嘤。

## A 迷宫

看了大佬们的博客感觉好像明白了于昊的做法

先bfs搜出最大的深度和每一层点的个数

每一层只要有两个或以上的点就会使这些人在某一点冲突，会多走人数减1步，但是如果没有点就可以少走一步，如果多走的步数为0，则不需要少走。

之后就可以按每个点都可以装下无限个数的人来计算，整个图畅通，不会堵住，计算步数，也就是最深的点的深度。

多走的步数加上不多走时需要的步数就是答案。

```c++
#include<bits/stdc++.h>
using namespace std;
int n,m,to[200010],ne[200010],fi[200010],tot,mm,nn,a[100001],d[100001],v[100001],n1,n2,mark[100001],dmax,ans;
queue<int> q;
//mark[i] 表示深度为i的节点中1的个数；d[i]表示i节点的深度； 
int add(int x,int y){
    to[++tot]=y;
    ne[tot]=fi[x];
    fi[x]=tot;
    return 0;
}
int main()
{
    cin>>n;
    for(int i=1;i<=n;i++)
        scanf("%d",&a[i]);
    for(int i=1;i<n;i++)
    {
        scanf("%d%d",&n1,&n2);
        add(n1,n2);
        add(n2,n1);
    }
    d[1]=0;
    v[1]=1;
    q.push(1);
    while(!q.empty())
    {
        int x=q.front();
        q.pop();
        for(int i=fi[x];i;i=ne[i])
        {
            int y=to[i];
            if(!v[y]){
                q.push(y);
                d[y]=d[x]+1;
                if(a[y]){
                    mark[d[y]]++;
                    dmax=max(dmax,d[y]);
                }
                v[y]++;
            }
        }
    }
    for(int i=1;i<=dmax;i++)
    {
        ans+=mark[i]-1;
        if(ans<0)ans=0;
    }
    ans+=dmax;
    cout<<ans;
    return 0;
}
```

## B 重新定义字典序

考虑枚举 **k**，使得 A,B 的第 **p[1],p[2]…p[k-1]** 小都相等，但 是 B 的第 **p[k]** 小比 A 的第 **p[k]** 小要小

可以枚举 B 的第 **p[k]** 小的值，这样是 **n^2** 的 

分情况枚举可以做到 nlogn

## C 斐波那契数列

 **F[n]&(F[n]-1)=F[n]-lowbit(F[n])**

而只有 n 是 3 的倍数时，**F[n]** 才是偶数

当 **n=3k** ，时，若 k 是奇数，则 **lowbit(F[n])=2** ，否则  **lowbit(F[n])=lowbit(4k)**

矩阵乘法求⼀一下 F 的前缀和

## D 二次函数

 对二次函数进行行平移，使得 0<=a<=1
1. 讨论1：a 全是 0
2. 讨论2：存在两个 a，⼀一个是 0，⼀一个是 1
3. 讨论3：a 全是 1

## E 线性探查法

div2 建图再拓扑排序，临时学了拓扑排序，但是建图没有建对，看了题解后，用了新的建图方法，然而。。。。。

wa了五页

我以为我建图没建对，搞了半天还是wa，我又以为我拓扑排序没写对，结果还是wa。。。

最后。。。

我把数组大小改了一下，令 **N=1005** ，邻接表的大小为 **N×N** ，竟然过了。。

过了。

过了

（已自闭

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1010,M=998244353;//N=100005时wa了五页
int Next[N*N],head[N*N]={0},ver[N*N],tot=0;
void add(int x,int y)
{
    tot++;
    ver[tot]=y;
    Next[tot]=head[x];
    head[x]=tot;
}
struct node{
    int p;
    ll vv;
    friend bool operator <(const node &n,const node &m)
    {
        return n.vv>m.vv;
    }
};
ll a[N];
ll in[N]={0};
ll ans[N];
int main()
{
    int n;
    ios::sync_with_stdio(false);
    cin>>n;
    priority_queue<node> Q;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    for(int i=1;i<=n;i++)
    {
        int h=(ll)a[i]%n+1;
        if(h==i){
            Q.push({i,a[i]});
        }
        if(h<i){
            for(int j=i-1;j>=h;j--)
            {
                add(j,i);
                in[i]++;
            }
        }
        else if(h>i){
            for(int j=1;j<=i-1;j++)
            {
                add(j,i);
                in[i]++;
            }
            for(int j=h;j<=n;j++)
            {
                add(j,i);
                in[i]++;
            }
        }
    }
    int t=1;
    while(!Q.empty())
    {
        node c=Q.top();
        ans[t]=c.vv;
        Q.pop();
        t++;
        int x=c.p;
        for(int i=head[x];i;i=Next[i])
        {
            int y=ver[i];
            in[y]--;
            if(in[y]==0){
                node d;
                d.p=y;d.vv=a[y];
                Q.push(d);
            }
        }
    }
    cout<<ans[1];
    for(int i=2;i<=n;i++)
    {
        cout<<' '<<ans[i];
    }
    cout<<endl;
}
```

## F 逆序对！

上午的 div1 讲课的课堂检测
$$a^{S}>b^{S}$$ 的条件是，设 i 是 $$a^{b}$$ 的最高位，**a** 的第 **i** 位要 与 **b** 的第 **i** 位相同
暴暴力力是 **n^2** 枚举所有对，最后对每位统计⼀一下答案
把暴暴力力换成分治可以变成 **nlogn**

## G 抢红包机器人

每个人都可能是机器人，每个机器人前的人都是机器人。

这不就是张图吗，建图，枚举每个点，深搜n次，搞定。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=100005,M=998244353;
int a[105][105];
int v[105];
int Next[N],head[N]={0},ver[N],tot=0;
void add(int x,int y)
{
    tot++;
    ver[tot]=y;
    Next[tot]=head[x];
    head[x]=tot;
}
int h[105];
int ans=0;
void dfs(int p)
{
    for(int i=head[p];i;i=Next[i])
    {
        int y=ver[i];
        if(v[y])continue;
        ans++;
        v[y]=1;
        dfs(y);
    }
}
int main()
{
    int n,m;
    ios::sync_with_stdio(false);
    cin>>n>>m;
    for(int i=1;i<=m;i++)
    {
        cin>>h[i];
        for(int j=1;j<=h[i];j++)
        {
            cin>>a[i][j];
        }
    }
    for(int i=1;i<=m;i++)
    {
        for(int j=1;j<=h[i];j++){
            for(int k=j+1;k<=h[i];k++)
            {
                add(a[i][k],a[i][j]);
            }
        }
    }
    int minn=1e9;
    for(int i=1;i<=n;i++)
    {
        ans=1;
        memset(v,0,sizeof(v));
        v[i]=1;
        dfs(i);
        minn=min(ans,minn);
    }
    cout<<minn<<endl;
}
```

## H 同构

取补图，每个点度数变成 2，于是图就是若⼲干个⼤大于等于 3 的环构成的，答案相当于 **n** 的整数划分

可以用一个 **n^1.5** 的 **dp** 过掉

## I 集合

假设 **T** 分成了了 $$A+B$$ ，设 **A,B** 的生成函数分别为$$a(x),b(x)$$

那么$$f(A)+f(B)=a(x)^{3}+b(x)^{3}$$的 $$x^{n},x^{2n},x^{3n}$$ 的系数之和

$$a(x)^{3}+b(x)^{3}=(a(x)+b(x))(a(x)^{2}-a(x)b(x)+b(x)^{2})$$

$$a(x)+b(x)$$是全集，对于右边多项式的任意⼀一个系数，$$a(x)+b(x)$$ 只存在一个 $${x}^{i}$$ 和他乘起来后是 **n** 的倍数

所以 $$f(A)+f(B)=|A|^{2}-|A|(n-|A|)+(n-|A|)^{2}$$

## J 强壮的排列

设答案是 **f(n)**

考虑最⼤大的数的位置是 **i**，则变成一个长度为 **i-1** 的数列列和 一个长度为 **n-i** 的数列列

所以 **f(n)=sum(f(i)f(n-i-1))/n**

所以 **f(x)’=f(x)^2+1**

解得 **f(x)=tan(x)**
