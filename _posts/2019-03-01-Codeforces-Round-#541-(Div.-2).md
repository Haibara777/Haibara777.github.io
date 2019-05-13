---
layout: post
title:  "Codeforces Round #541 (Div. 2)"
date:   2019-03-01
desc: "Codeforces Round #541 (Div. 2)"
keywords: "Codeforces"
categories: [Acm]
tags: [Codeforces]
icon: icon-html
---

[比赛链接](http://codeforces.com/contest/1131)

>这场我又自闭了233333，以后打cf两条禁忌：
>
>1. 睡眠不好时不打
>2. 周围环境嘈杂时不打
>
>我tmd在这上面吃两次亏了，上次是读错题，这次是忘了删debug的那一行，我以为我做错了，我。。。。算了，自闭了。。。。

## [A. Sea Battle](http://codeforces.com/contest/1131/problem/A)

做这个题的时候人还没来，环境比较安静，没遇到什么困难

```c++
#include<iostream>
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
#define ll long long
using namespace std;
const int N=1e4+10,M=1e5+3;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
int main()
{
    ios::sync_with_stdio(false);
    ll w1,w2,h1,h2;
    cin>>w1>>h1>>w2>>h2;
    ll ans=0;
    ans+=w1+2;
    ans+=h1*2;
    ans+=w2+2;
    ans+=h2*2;
    ans+=w1-w2;
    cout<<ans<<endl;
}
```

## [B. Draw!](http://codeforces.com/contest/1131/problem/B)

从这开始就炸了，吵得一匹，瞬间爆炸，最后勉强把这个题钢过去了

```c++
#include<iostream>
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
#define ll long long
using namespace std;
const int N=1e4+10,M=1e5+3;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
ll a[N];
ll b[N];
ll ss(ll x1,ll y1,ll x2,ll y2){
    if(x1==y1)
    if(x1==y1&&x2==y2)return 0;
    ll ans=min(y1,y2)-max(x1,x2);
    if(y1<x2||y2<x1)return 0;
    if(x1!=x2)ans++;
    return ans;
}
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
        cin>>a[i]>>b[i];
    ll ans=min(a[1],b[1])+1;
    for(int i=2;i<=n;i++)
        ans+=ss(a[i-1],a[i],b[i-1],b[i]);
    cout<<ans<<endl;
}
```

## [C. Birthday](http://codeforces.com/contest/1131/problem/C)

我做出来了啊！！！！没交上啊！！！！刚读完题思路是对的，但是一会思路就忘了，后来好不容易找回来，但是。。。

艹。。。。

我再在禁忌情况下打cf，请打死我

因为一开始思路跑偏，所以代码有点奇怪，所以重写了一份，思路一样，比较精简

```c++
#include<iostream>
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
#define ll long long
using namespace std;
const int N=2e2+10,M=1e5+3;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
int a[N];
int ans[N];
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
        cin>>a[i];
    sort(a+1,a+n+1);
    int l=1,r=n;
    for(int i=1;i<=n;i++)
    {
        if(i%2==1)ans[l++]=a[i];
        else ans[r--]=a[i];
    }
    for(int i=1;i<=n;i++)
    {
        cout<<ans[i];
        cout<<((i==n)?'\n':' ');
    }
}
```

## [D. Gourmet choice](http://codeforces.com/contest/1131/problem/D)

这道题。。。。并查集加拓扑排序。。。。

一开始并没有想到是这么做，毕竟拓扑排序就做过一道（自闭那八天做的）。。。

但是后来想想好像真是这么回事，给出相对大小不久相当于给出先后顺序嘛，等号就说明两个值相等嘛，当成一个点来处理就好了，有了先后顺序后，因为要求最大的数要尽量小，于是就只能拓扑排序

```c++
#include<iostream>
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
#define ll long long
using namespace std;
const int N=1e3+10,M=1e5+3;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
int head[2*N],ver[N*N],Next[N*N],tot=0;
void add(int x,int y)
{
    ver[++tot]=y;
    Next[tot]=head[x];
    head[x]=tot;
}
char a[N][N];
int fa[N*2];
int in[N*2];
int get(int x)
{
    if(fa[x]==x)return x;
    return fa[x]=get(fa[x]);
}
int ans[N*2];
int main()
{
    ios::sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
        {
            cin>>a[i][j];
        }
    for(int i=1;i<=n+m;i++)fa[i]=i;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
        {
            if(a[i][j]=='='){
                fa[get(n+j)]=get(i);
            }
        }
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
        {
            if(a[i][j]=='<')add(get(i),get(n+j)),in[get(n+j)]++;
            if(a[i][j]=='>')add(get(n+j),get(i)),in[get(i)]++;
        }
    queue<int> Q;
    for(int i=1;i<=n+m;i++)
    {
        if(fa[i]==i&&in[i]==0){
            ans[get(i)]=1;
            Q.push(get(i));
        }
    }
    while(!Q.empty())
    {
        int x=get(Q.front());
        Q.pop();
        for(int i=head[x];i;i=Next[i])
        {
            int y=get(ver[i]);
            in[y]--;
            if(in[y]==0){
                ans[y]=ans[x]+1;
                Q.push(y);
            }
        }
    }
    for(int i=1;i<=n+m;i++)
    {
        if(ans[get(i)]==0){
            cout<<"No"<<endl;
            return 0;
        }
    }
    cout<<"Yes"<<endl;
    for(int i=1;i<=n;i++)
    {
        cout<<ans[get(i)];
        cout<<((i==n)?'\n':' ');
    }
    for(int i=n+1;i<=n+m;i++)
    {
        cout<<ans[get(i)];
        cout<<((i==n+m)?'\n':' ');
    }
}
```

## [E. String Multiplication](http://codeforces.com/contest/1131/problem/E)

    unsolved

## [F. Asya And Kittens](http://codeforces.com/contest/1131/problem/F)

可以想到跟并查集有关，但是这道题有先后顺序，而且需要连出一条链，于是我们就根据先后顺序连出一条连来就行了

所以关键就是怎样连成一条链，因为一开始思路又跑偏了，所以直接去看了前几名的代码，一开始没看懂，看懂后恍然大悟，他用了一种非常巧妙的方法

因为要连成一条链，中途连接的过程中就会出现很多小链，所以读入的每两个数，一个找链首，一个找链尾，然后再连起来

而并查集只能找链首，于是他用了另一个并查集，始终与原并查集反着连，那么另一个并查集的链首就是原并查集的链尾

太机智了woc，不得不服啊

```c++
#include<iostream>
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
#define ll long long
using namespace std;
const int N=15e4+10,M=1e5+3;
int fal[N],far[N];
int ver[N];
int getr(int x)
{
    if(x==far[x])return x;
    return far[x]=getr(far[x]);
}
int getl(int x)
{
    if(x==fal[x])return x;
    return fal[x]=getl(fal[x]);
}
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
        fal[i]=far[i]=i;
    int x,y;
    for(int i=1;i<n;i++)
    {
        cin>>x>>y;
        x=getr(x),y=getl(y);
        ver[x]=y;
        far[x]=y;fal[y]=x;
    }
    x=getl(x);
    for(int i=1;i<=n;i++)
    {
        cout<<x<<((i==n)?'\n':' ');
        x=ver[x];
    }
}
```

## [G. Most Dangerous Shark](http://codeforces.com/contest/1131/problem/G)

    unsolved


>原本打完这场后的第二天就该更新了来着，但是。。。。咕了一周。。。
>
>毕竟快开学了嘛，完全没有学习的欲望呜呜呜