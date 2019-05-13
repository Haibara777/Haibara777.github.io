---
layout: post
title:  "Codeforces Round #538 (Div. 2)"
date:   2019-02-12
desc: "Codeforces Round #538 (Div. 2)"
keywords: "Codedorces"
categories: [Acm]
tags: [Codedorces]
icon: icon-html
---



>开开心心来到慢城，待了一下午，打算看完CSAPP的第二章
>
>想到上次CF的C题还没做出来，补完再看吧。。
>
>等我补完E题的时候：我今天下午主要想干啥来着。。。
>
>艹。。。
>
>对不起，又咕了。。。

## A. Got Any Grapes?

直接贴代码。。。

没有问题。。。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5,M=1e9+7;
int a[N];
int main()
{
    ios::sync_with_stdio(false);
    int x,y,z;
    int a,b,c;
    cin>>x>>y>>z;
    cin>>a>>b>>c;
    int flag=0;
    if(a>=x)
    {
        int bb=a+b-x;
        if(bb>=y)
        {
            int cc=bb+c-y;
            if(cc>=z)flag=1;
        }
    }
    if(flag)cout<<"YES"<<endl;
    else cout<<"NO"<<endl;
}
```

## B. Yet Another Array Partitioning Task

一开始想多了，其实第m*k大数就算有重复的也不会对答案有任何影响。。。

于是找出前m*k大数，然后从左到右分成k组数，每组数恰好有前m*k大数中的m个数就行了

结构体随便一排，随便一分就ok了

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=2e5+5,M=1e9+7;
struct node{
    int p;
    ll v;
}a[N];
bool cmp1(node n,node m)
{
    return n.v>m.v;
}
bool cmp2(node n,node m)
{
    return n.p<m.p;
}
int main()
{
    ios::sync_with_stdio(false);
    int n,k,m;
    cin>>n>>m>>k;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i].v;
        a[i].p=i;
    }
    sort(a+1,a+n+1,cmp1);
    int tot=m*k;
    ll ans=0;
    for(int i=1;i<=tot;i++)
    {
        ans+=a[i].v;
    }
    cout<<ans<<endl;
    sort(a+1,a+tot+1,cmp2);
    cout<<a[m].p;
    for(int i=2*m;i<m*k;i+=m)
    {
        cout<<' '<<a[i].p;
    }
    cout<<endl;
}
```

## C. Trailing Loves (or L'oeufs?)

这个题做到最后还是没能做出来，虽然想出了大部分的思路，但是一些处理还是没有想出来

一个数在k进制下后面0的个数取决于这个数除以 **k** 能有几次余数为0，换句话说，就是这个数的因子最多能出现几个 **k**

但是这个处理方式。。。

一开始我是想求出 **k** 的每一对因子，求每对因子在阶乘中出现的次数，即两个因子出现次数的较小值，但是。。。wa了，因为后面的因子的因子可能会包含前面的因子

于是这个题最后也没能写出来。。。

后来看了看rank1的代码，跟我的代码结合一下，总算是过了这个题

其实这个题应该求k的所有因子（包括重复的因子）在n中出现的次数的最小值。。。艹

所以找到 **k** 的一个因子 **i** 后， **i** 在 **k** 中出现了几次，最后 **i** 出现的次数就要分成几份，因为重复的因子也要算

每算完一个因子，都要将这个因子在 **k** 中去掉

最后如果 **k** 不是1，还要将 **k** 本身算一遍

没想出来啊。。。。可惜

```c++
#include<bits.stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5,M=1e9+7;
int main()
{
    ios::sync_with_stdio(false);
    ll n,k;
    cin>>n>>k;
    ll ans=1e18;
    for(ll i=2;i*i<=k;i++)
    {
        if(k%i==0)
        {
            ll x=i;
            ll num=n/x;
            ll h=num;
            ll cnt=0;
            while (k % i == 0)
            {
                k /= i;
                cnt++;
            }
            while(h!=0){
                num+=h/x;
                h/=x;
            }
            ans=min(ans,num/cnt);
        }
    }
    if(k!=1){
        ll num=n/k;
        ll h=num;
        while(h!=0){
            num+=h/k;
            h/=k;
        }
        ans=min(ans,num);
    }
    cout<<ans<<endl;
}
```

## D. Flood Fill

这个题果然没有我一开始想的那么简单，先把连续重复的元素去掉，只留下一个就行，多出来的没有任何用

处理后，本来是想用元素总个数减去出现次数最多的元素的个数就行，但是忽略了一点，如果两个相同元素之间还有两个相同的元素，中间两个相同元素也会减少操作次数，于是我就去看大佬们代码了。。。

然后找到了一个非常好理解的dp

只需要开一个 **dp\[ l \]\[ r \]** ，代表操作区间 **( l , r )** 需要的总共的操作次数

在不考虑区间两端元素相同的情况的时候，答案就是 **( l + 1 , r )** 的操作次数和 **( l , r - 1 )** 的操作次数的较小值（因为后面还要考虑两端元素相同的情况，所以这两个值可能不相等）加一

经过以上操作后，如果恰好区间两端的元素相同，那么操作次数就是 **( l + 1, r - 1 )** 的操作次数加一，一般是比 **( l , r )** 的操作次数小吧，以防万一取一个min

于是就可以愉快的dp了，最终答案就是 **dp\[ 1 \]\[ n \]**

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=5e3+5,M=1e9+7;
int a[N];
int c[N];
int dp[N][N];
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    int pre=-1;
    int cnt=0;
    for(int i=1;i<=n;i++)
    {
        if(pre==a[i])continue;
        c[++cnt]=a[i];
        pre=a[i];
    }
    for(int r=1;r<=cnt;r++)
    {
        for(int l=r;l>=1;l--)
        {
            dp[l][r]=1e8;
            if(l==r){
                dp[l][r]=0;
                continue;
            }
            dp[l][r]=min(dp[l+1][r],dp[l][r-1])+1;
            if(c[r]==c[l]&&l+1<=r-1){
                dp[l][r]=min(dp[l][r],dp[l+1][r-1]+1);
            }
        }
    }
    cout<<dp[1][cnt]<<endl;
}
```

## E. Arithmetic Progression

这个交互题对我来说太鬼畜了，题意读的我一脸懵逼。。。

第一次遇到交互题，输入输出都看不懂。。。

不过还好有题解

题意是有一个长度为 **n** 的乱序的数列，如果递增排列的话是一个等差数列，让你用以下询问在60次内求出这个等差数列：

1. **" ? i "** ，可以询问第 **i** 个数的具体大小
2. **" > x "** ，可以检查数列中是否有严格大于 **x** 的数，有就输出 1 ，没有就输出 0

以上询问需要程序输出，测评姬给出询问的答案。。。所以题意已读懵。。。

直接用第二个询问在 **\[ 1 , 1e9 \]** 范围内二分查找整个数列中的最大值，然后用剩下的询问次数用第一个询问随机求出一些值，用最大值与他们作差，求所有差值的 **gcd** 就是公差 **d** 

因为要输出首项和公差，首项就是最大值减去 **( n - 1 ) \* d**

这么做答案错误的概率极小，所以可以认为是正确的

但是随机数把我坑了好久，**rand()** 能生成的随机数好像有一定范围，所以要使用 **rand() \* rand()** 。。。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e6+5,M=1e9+7;
int v[N];
int a[100];
int main()
{
    ios::sync_with_stdio(false);
    int l=0,r=1e9;
    int cnt=0;
    int n;
    cin>>n;
    while(l<r){
        cnt++;
        int mid=l+((r-l)>>1);
        cout<<"> "<<mid<<endl;
        cout.flush();
        int flag;
        cin>>flag;
        if(flag){
            l=mid+1;
        }
        else r=mid;
    }
    int nn=min(n,60-cnt);
    int d=0;
    for(int i=1;i<=nn;i++)
    {
        int x=rand()*rand()%n+1;
        while(v[x])x=rand()*rand()%n+1;
        v[x]=1;
        cout<<"? "<<x<<endl;
        cout.flush();
        cin>>x;
        d=__gcd(d,l-x);
    }
    cout<<"! "<<l-(n-1)*d<<' '<<d<<endl;
}
```

## F. Please, another Queries on Array?

救命啊，这个硬核线段树我写不出来啊嘤嘤嘤嘤嘤嘤嘤嘤嘤。。。。。