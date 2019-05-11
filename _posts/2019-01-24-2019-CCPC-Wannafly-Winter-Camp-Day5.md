---
layout: post
title:  "2019 CCPC Wannafly Winter Camp Day5"
date:   2019-01-24
desc: "2019 CCPC Wannafly Winter Camp"
keywords: "Camp"
categories: [Acm]
tags: [2019 CCPC Wannafly Winter Camp]
icon: icon-html
---

_____

今天题目还好，这两天的体验比以前好了不少，可能是因为讲师团察觉到我们很菜了吧，A、C两题做的比较快，H题想的有点多，div2版本其实直接建树就可以了，结果想到div1的方法去了，这我绝对百分之百想不出来啊，还好最后试了一发，侥幸过了。

## A Cactus Draw

div1：仙人掌，把仙人掌中的环横着放。

div2：一棵树，快乐的一匹。
```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=100005;
ll a;
int main()
{
    ios::sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    int num=0;
    priority_queue<ll> Q;   
    for(int i=1;i<=n;i++)
    {
        cin>>a;
        num+=log(a)+1;
        Q.push(a);
    }
    if(k>num){cout<<'0'<<endl;return 0;}
    for(int i=1;i<=k;i++)
    {
        a=Q.top();
        Q.pop();
        a/=2;
        Q.push(a);
    }
    ll ans=0;
    while(!Q.empty())
    {
        ans+=Q.top();
        Q.pop();
    }
    cout<<ans<<endl;
}
```

## B Diameter

    unsolved

## C Division

div1：主席树，各凭本事卡内存。（完全听不懂）

div2：又是快乐的一匹。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
ll a;
int main()
{
    ios::sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    int num=0;
    priority_queue<ll> Q;   
    for(int i=1;i<=n;i++)
    {
        cin>>a;
        num+=log(a)+1;
        Q.push(a);
    }
    if(k>num){cout<<'0'<<endl;return 0;}
    for(int i=1;i<=k;i++)
    {
        a=Q.top();
        Q.pop();
        a/=2;
        Q.push(a);
    }
    ll ans=0;
    while(!Q.empty())
    {
        ans+=Q.top();
        Q.pop();
    }
    cout<<ans<<endl;
}
```

## D Doppelblock

爆搜+剪枝
剪枝：
1. 数不重复。
2. 两个X之外的和不超过数的总和减线索值，两个X之内的和不超过线索。
3. 两个X之间能放的数字个数被线索值限制。

## E Fast Kronecker Transform

暴力

求一个奇怪的卷积。

FFT

## F Kropki

与汉密尔顿路径的状压dp相似。

考虑容斥，

dp[]

>40的无序拆分大约30000左右，50大约200000左右。

## G Least Common Multiple

    unsolved

## H Nested Tree

div1： 
1. 把所有树串串起来之后，某些边的两端的大小会变。
2. 我们可以⽤用虚树/树链剖分解决。(我又不会emmmm)

div2：可以把整个树建出来，对于⼀一条边，对答案的贡献 是两端size乘积的和（上网搜了个dfs过了）。
```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=2000005;
const int mod=1e9+7;
int vis[N]={0};
int Next[N],head[N],ver[N],tot=0;
int n,m;
void add(int x,int y)
{
    tot++;
    ver[tot]=y;
    Next[tot]=head[x];
    head[x]=tot;
}
ll ans=0;
void dfs(ll u,ll nu){
    ll num=nu;
    for(ll i=head[u];i;i=Next[i]){
        ll v=ver[i];
        if(vis[v]) continue;
        vis[v]=1;
        tot++;
        dfs(v,tot);
        ans=(ll)(ans+((n*m-(tot-num))*(tot-num)%mod)%mod)%mod;
        num=tot;
    }
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>n>>m;
    for(int i=1;i<=n-1;i++)
    {
        int u,v;
        cin>>u>>v;
        for(int j=0;j<=m-1;j++)
        {
            add(u+j*n,v+j*n);
            add(v+j*n,u+j*n);
        }
    }
    for(int i=1;i<=m-1;i++)
    {
        int a,b,v,u;
        cin>>a>>b>>u>>v;
        add(u+(a-1)*n,v+(b-1)*n);
        add(v+(b-1)*n,u+(a-1)*n);
    }
    ans=0;tot=1;vis[1]=1;
    dfs(1,1);
    cout<<ans%mod<<endl;
}
```


## I Sorting

区间中大于x的数字相对位置不变，小于等于x的数字也不会变。

把<=x的数字看为0，>x的看成1。

可以通过找第几个0或1找到原来的数字。

1，维护区间里有几个0几个1。
2，区间求和就相当于搞出这是第几个0到第几个0，第几个1到第几个1，然后前缀和即可。

## J Special Judge

没有公共端点就是不规范相交。
有就判断是否是一个方向。
一个方向重合则不合法。
用叉积=0，点积>0。
