---
layout: post
title:  "2019 CCPC Wannafly Winter Camp Day8"
date:   2019-01-27
desc: "2019 CCPC Wannafly Winter Camp"
keywords: "Camp"
categories: [Acm]
tags: [2019 CCPC Wannafly Winter Camp]
icon: icon-html
---

总算是撑到了最后一天，本以为题今天会好点，结果又自闭了。。。。。

## A Aqours

    unsolved

## B 玖凛两开花

    unsolved

## C 御坂妹妹

    unsolved

## D 吉良吉影的奇妙计划

    unsolved

## E Souls-like Game

    unsolved

## F 地球上最漫长的一天

    unsolved

## G 穗乃果的考试

这就是个傻\*\*\*\*\*\*\*题啊，艹艹艹艹艹艹

这个公式
<br />

$$
\sum_{i=0}^{nm}i*f(i)
$$

<br />
就是这个公式，一开始我以为算出每个 **f(i)** 就好，结果就没跳出这个坑，啊啊啊啊啊啊啊！！！！！！

后来仔细想一下，**f(i)** 是有 **i** 个1的子矩阵的个数，所以 **i f(i)** 
就是每个子矩阵的每个 1 都对答案有 1 的贡献，所以每个 1 每在一个子矩阵里，答案就加一。

所以最后就算每个 1 在几个子矩阵里，然后相加就好了。

至于 1 在几个子矩阵里怎么算。。。用力想一下就好了啊，总公式就是
 **i\*(n-i+1)\*j\*(m-j+1)** 。

>还好多想了想，不然就爆零了

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=2005,M=998244353;
//int a[N];
char a[N][N];
int main()
{
    ios::sync_with_stdio(false);
    ll n,m;
    cin>>n>>m;
    for(ll i=1;i<=n;i++)
    {
        for(ll j=1;j<=m;j++)
            cin>>a[i][j];
    }
    ll ans=0;
    for(ll i=1;i<=n;i++)
    {
        for(ll j=1;j<=m;j++)
        {
            if(a[i][j]=='1'){
                ans+=(ll)(i*(n-i+1)%M)*(j*(m-j+1)%M);
                ans%=M;
            }
        }
    }
    cout<<ans%M<<endl;
}
```

## H 二人的白皇

    unsolved

## I 岸边露伴的人生经验

    unsolved

## J 去音乐会

    unsolved
