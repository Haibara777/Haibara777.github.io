---
layout: post
title:  "Codeforces Global Round 1"
date:   2019-02-13
desc: "Codeforces Global Round 1"
keywords: "Codedorces"
categories: [Acm]
tags: [Codedorces]
icon: icon-html
---

# Codeforces Global Round 1
____

>这才是我那天想补的一套题啊，虽然昨天的线段树还没写完（那个题是真的硬核），但是还是要好好补这一套啊

## A. Parity

暴力就好了啊，反正1e5不会超时

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5;
int a[N];
int main()
{
    ios::sync_with_stdio(false);
    int b,k;
    cin>>b>>k;
    int n=0;
    for(int i=1;i<=k;i++){
        cin>>a[i];
    }
    int bb=1;
    for(int i=k;i>=1;i--)
    {
        n=(n+bb*a[i])%2;
        bb=(bb*b)%2;
    }
    if(n%2==1){
        cout<<"odd"<<endl;
    }
    else cout<<"even"<<endl;
}
```

## B. Tape

我隐约记得当时又想多了。。。

其实还是那个套路，先按一整条算，算出总长，再从中间断开，断开 **k - 1** 次就可以分成 **k** 段了，每次断开最长的一段就可以使最后剩下的最短

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+10;
int a[N];
int b[N];
int main()
{
    ios::sync_with_stdio(false);
    int n,m,k;
    cin>>n>>m>>k;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    int ans=a[n]-a[1]+1;
    for(int i=2;i<=n;i++)
    {
        b[i-1]=a[i]-a[i-1];
    }
    sort(b+1,b+n);
    for(int i=1;i<=k-1;i++){
        ans-=b[n-i];
        ans++;
    }
    cout<<ans<<endl;
}

```

## C. Meaningless Operations

这个题我在不停找规律，然后不停将其中一些情况按规律输出，但是剩下的那些还是用的暴力求出，所以还是超时

但是我最后发现，比 **n** 小的数中，只要能找到一个数 **i** ，使 **i & n = 0** ，那么最后算出来的结果一定比为 **n + i** ，而且如果不等于 **0** ，算出的结果一定比 **n** 小，所以就找最大的这种数就行了

而最大的这种数就是 **n** 按位取反，取与 **n** 同位数的一段就行了，而仔细观察后发现，找不到这种数的 **n** 因为受到题目限制，只有23个，所以我就写了程序跑了半天把这23个数算出来，然后打表莽过去了。。。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e6+1000;
const int a[30]={0,0,1,1,5,1,21,1,85,73,341,89,1365,1,5461,4681,21845,1,87381,1,349525,299593,1398101,178481,5592405,1082401,22369621};
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        int n;
        cin>>n;
        int ans=0;
        int kk=n;
        int k=0;
        while(kk){kk>>=1;k++;}
        int x=n^((1<<k)-1);
        if(x!=0){
            cout<<n+x<<endl;
            continue;
        }
        cout<<a[k]<<endl;
    }
}
```

## D. Jongmah

这个题一眼看去，显然是dp。。。

然后就不知道怎么做了。。。

作为一个dp弱鸡只能去看题解

因为三个相同的连续元素的三元组一定可以分成三个不同的重复元素的三元组，所以可以认为连续元素的三元组最多有两个

**dp\[i\]\[x\]\[y\]** 表示到i时，总共可以生成的三元组的最大数目，且这些三元组的右端点都小于等于 **i** ，并且之后可以形成 **x** 个 **[i - 1 , i , i + 1]** 的三元组，**y** 个 **[i , i + 1 , i + 2]** 的三元组

之后每到一个 **i** ，就把 **i - 1** 的 **[i - 2 , i - 1 , i]** 算进去，然后把 **i** 形成重复元素三元组的个数算进去

于是就可以愉快的dp了 wuwuwuwuwu

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e6+5,M=1e9+7;
int dp[N][3][3],a[N]={0};
int main()
{
    int n,m;
    ios::sync_with_stdio(false);
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        int k;cin>>k;
        a[k]++;
    }
    memset(dp,-0x3f,sizeof(dp));
    dp[0][0][0]=0;
    for(int i=1;i<=m;i++)
    {
        for(int x=0;x<3;x++)
            for(int y=0;y<3;y++)
                for(int z=0;z<3;z++)
                {
                    int h=a[i]-x-y-z;
                    if(h<0)continue;
                    dp[i][x][y]=max(dp[i][x][y],dp[i-1][z][x]+h/3+z);
                }
    }
    cout<<dp[m][0][0]<<endl;
}
```

## E. Magic Stones

代码好理解，差分一下，只要原数列开头相等，差分后两数列一一对应，就时Yes，否则就是No，但是为什么会这样呢

我们用其中三个连续的数 **a , b , c** 做例子，则差分的值 **x1=b-a** ， **y1=c-b** ，将 **b** 操作变为 **a+c-b** 后，再次进行差分， **x2=(a+c-b)-a=c-b** ，**y2=c-(a+c-b)=b-a** ，此时，你会惊奇的发现， **x1=y2**，**x2=y1** 

所以每次操作只会交换两个差分值，只要两数列的开头或结尾相同，差分值一一对应，就一定可以变成同一个数列

>不知道为什么又玄学了一发，同一份代码交了两边才过，第一次莫名其妙wa，本地也是对的，再交一遍就过了。。。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5;
ll a[N],b[N];
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    ll now,pre1,pre2;
    cin>>pre1;
    for(int i=1;i<n;i++){
        cin>>now;
        a[i]=now-pre1;
        pre1=now;
    }
    cin>>pre2;
    for(int i=1;i<n;i++){
        cin>>now;
        b[i]=now-pre2;
        pre2=now;
    }
    sort(a+1,a+n);
    sort(b+1,b+n);
    int flag=0;
    for(int i=1;i<n;i++)
        if(a[i]!=b[i])flag=1;
    if(pre1!=pre2)flag=1;
    if(flag)cout<<"No"<<endl;
    else cout<<"Yes"<<endl;
}
```

