---
layout: post
title:  "2019 CCPC Wannafly Winter Camp Day3"
date:   2019-01-22
desc: "2019 CCPC Wannafly Winter Camp"
keywords: "Camp"
categories: [Acm]
tags: [2019 CCPC Wannafly Winter Camp]
icon: icon-html
---


今天又是自闭的一天，不过今天终于有了签到题，体验比前两天好了不少，并从数论题上学到了一些东西。
>顺便每个题把老师的一句话题解贴上了

## A 二十四点*

搜+打表，370个无解集合。

    unsolved

## B 集合

分类讨论，反射，二分求出入射角等于反射角的值，**O(Tlogn)**

## C 小游戏

**DP,O(nlogn)**

    unsolved

## D 精简改良

状压 **DP,O(3^nn^2)** 

    unsolved

## E 最大独立集

贪心， **O(n)**

    unsolved

## F 小清新数论*

>搞完签到题后一直在搞这一道题，但是可惜，最后还是没能做出来，但是做了一下午没做出来反而是我今天最大的收获，下面就写写我下午做这道题的经过吧。

这道题乍一看莫比乌斯函数就被吓了一跳，但是为了克服对数论的恐惧，只能硬着头皮上，头铁了一下午，总算是对莫比乌斯函数有了些了解。

先说说我的思路，一开始我是认为这道题跟莫比乌斯反演有很大关系，虽然现在看看确实如此，但是我最终做出来的时候，并没有用到莫比乌斯反演。一开始去看了很多莫比乌斯反演的博客，尝试性得将公式化简了一下，但是没有成功。不过，这为我后来的化简提供了一些思路。

首先，由枚举 **i , j** 改为枚举 **d** 。
<br />

$$
    \sum_{i=1}^{n}{\sum_{j=1}^{n}{\mu(\gcd(i,j))}}=\sum_{d=1}^{n}\sum_{i=1}^{n}\sum_{j=1}^{n}{\mu(d)[\gcd(i,j)=d]}
$$

<br />
后面的公式又可以写成
<br />

$$
\sum_{d=1}^{n}\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}{\mu(d)[\gcd(i,j)=1]}=
\sum_{d=1}^{n}{\mu(d)}\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}[\gcd(i,j)=1]
$$

<br />
其实到这里我就不会做了，因为我只找到了求 **μ(n)** 的板子，后面的完全无法下手，所以我晚上要来了庄学姐的代码，发现他用了下面这个公式就可以 **O(n)** 求出结果（我总觉得他是感性认知出来的，因为我就是靠感性证明的）。
<br />

$$
\sum_{i=1}^{n}\sum_{j=1}^{n}[\gcd(i,j)=1]=2\sum_{i=1}^{n}\sum_{j=1}^{n}\phi(j)+1  (\phi(1)=0)\tag1 
$$

<br />
为什么会这样呢，我用了一个感性认知的方法来证明
我们以 **n=5** 的 **gcd(i,j)** 为例子
<br />

$$
\begin{matrix}
0&1&2&3&4&5\\
1&1&1&1&1&1\\
2&1&2&1&2&1\\
3&1&1&3&1&1\\
4&1&2&1&4&1\\
5&1&1&1&1&5\\
\end{matrix}
$$

<br />
显然它是关于从 **(1,1)** 到 **(5,5)** 的直线对称的，如果只考虑半边，第 **i** 行中 **gcd** 的值为 1 的个数为 **φ(i)** ，特殊的，令 **φ(1)=0** ，则 **φ(1)+φ(2)+...+φ(5)** 就是其中一个半边中的 **gcd** 为 1 的数的个数，乘上二再加对称轴上的一个 1 ，就是 **(1)**式。

最后，靠着学长网上找的线性筛的板子，总算是做出了这道题。

>怎么说呢，这道题还是没有真正明白，不管是杜教筛还是莫比乌斯反演都没有学会，之后我会再更新关于他们的博客，并把链接贴到这里。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const ll N=1e7+10,M=998244353;
ll phi[N+10];
bool vis[N+10];
ll prime[N+10];
ll miu[N+10];
ll tot;
void init(){
    miu[1]=1;
    tot = 0;
    for(ll i = 2; i <=N; i ++)
    {
        if(!vis[i])
        {
            prime[tot ++] = i;
            phi[i] = i - 1;
            miu[i]=-1;
        }
        for(ll j = 0; j < tot; j ++)
        {
            if(i * prime[j] >= N) break;
            vis[i * prime[j]] = 1;
            if(i % prime[j] == 0)
            {
                miu[i*prime[j]]=0;
                phi[i * prime[j]] = phi[i] * prime[j];
                break;
            }
            else
            {
                phi[i * prime[j]] = phi[i] * phi[prime[j]];
                miu[i*prime[j]]=-1*miu[i];
            }
        }
    }
}
int main()
{
    int n;
    ios::sync_with_stdio(false);
    cin>>n;
    init();
    ll ans=0;
    for(int i=1;i<=N;i++){
        phi[i]=(phi[i]+phi[i-1])%M;
    }
    for(int i=1;i<=n;i++)
    {
        int h=n/i;
        ans+=(ll)(10*M+miu[i]*(2*phi[h]+1)%M);//因为miu有负数，所以要加10*M
        ans%=M;
    }
    cout<<ans%M<<endl;
}
```
## G 排列

签到题，能读懂题意就能做出来。
```c++
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#define ll long long
using namespace std;
const int N=100005;
int a[N]={0};
int pa[N];
int ans[N];
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
        cin>>a[i];
    for(int i=1;i<=n;i++)
    {
        pa[a[i]]=i;
    }
    int minn=1e9;
    for(int i=1;i<=n;i++)
    {
        if(minn>pa[i]){
            minn=pa[i];
        }
        else if(pa[i]>=minn)pa[i]=minn;
    }
    int pre=pa[n];
    int h=1;
    for(int i=n;i>=1;i--)
    {
        if(pa[i]!=pre){
            h++;
            pre=pa[i];
        }
        pa[i]=h;
    }
    int p=pa[1];
    ans[1]=p;
    int p1=p+1;
    for(int i=2;i<=n;i++)
    {
        if(pa[i]<p){
            ans[i]=pa[i];
            p=pa[i];
            continue;
        }
        ans[i]=p1;
        p1++;
    }
    cout<<ans[1];
    for(int i=2;i<=n;i++)
        cout<<' '<<ans[i];
    cout<<endl;
}
```

## H 涂鸦*

贪心， **O(n)**

    unsolved

## I 石头剪刀布

并查集， **O(na(n))**

    unsolved

## J 子序列

矩阵乘法， **O(m^3n)**， **m**是字符集大小

    unsolved




