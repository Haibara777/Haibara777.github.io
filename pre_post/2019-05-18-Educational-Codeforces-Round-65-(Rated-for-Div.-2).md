---
layout: post
title:  "Educational Codeforces Round 65 (Rated for Div. 2)"
date:   2019-05-18
desc: "Educational Codeforces Round 65 (Rated for Div. 2)"
keywords: "Codeforces"
categories: [Acm]
tags: [Codeforces]
icon: icon-html
---

[Educational Codeforces Round 65 (Rated for Div. 2)](https://codeforces.com/contest/1167)

前面四题比较水，就不写了2333

## [E. Range Deleting](https://codeforces.com/contest/1167/problem/E)

。。。这个题的细节有些多，如果$$f( l_0 , r_0 )$$是合法的，那么对于所有的 $$l$$ 小于 $$l_0$$ ， $$r$$ 大于 $$r_0$$ ，$$f( l , r )$$都是合法的

而对于没有去掉的部分，对于每个数值x，小于x的数最后出现的位置一定在x第一次出现位置的前面，大于x的数第一次出现的位置一定在x最后一次出现的位置的后面，因为去掉了l到r的数值，可以将数值分为两部分考虑，1到l的数值最后一次出现的位置一定在r到x第一次出现位置的前面

所以先预处理每个数值第一次和最后一次出现的位置，还有前1到l的数值最后一次出现的位置，和r到x第一次出现的位置

然后可以用双指针，二分，或树状数组来做，就是我写双指针把自己心态写炸了。。

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
#include<bitset>
#define ll  long long
#define ull unsigned long long
using namespace std;
const int N=1e6+5,M=10007;
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
int s[N],t[N];
int ss[N],tt[N];
int main()
{
    ios::sync_with_stdio(false);
    int n,x;
    cin>>n>>x;
    memset(s,0x3f,sizeof(s));
    for(int i=1;i<=n;i++){
        int k;
        cin>>k;
        s[k]=min(s[k],i);
        t[k]=i;
    }
    memset(ss,0x3f,sizeof(ss));
    for(int i=1;i<=x;i++){
        tt[i]=max(tt[i-1],t[i]);
    }
    ss[x+1]=0x3f3f3f3f;
    for(int i=x;i>=1;i--){
        ss[i]=min(ss[i+1],s[i]);
    }
    ll ans=0;
    int r=x;
    while(ss[r]>t[r-1]&&r>=1)r--;
    for(int l=0;l<x;l++){
        if(tt[l-1]>s[l]&&l>0)break;
        while(ss[r]<tt[l]||r<=l+1)r++;
        ans+=x-r+2;
    }
    cout<<ans<<endl;
}
```

## [F. Scalar Queries](https://codeforces.com/contest/1167/problem/F)

差一点，当时都把离散化和树状数组写上去了，但是推的时候出了些问题

其实应该分别考虑每个数的贡献，从一个数找它所在的所有区间，这样就比较好考虑，但当时我是从找所有区间开始，导致没能做出来。。

这个题后来也wa了不少次，原因是树状数组没有取模。。。本以为爆不了来着。。。

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
#include<bitset>
#define ll  long long
#define ull unsigned long long
using namespace std;
const int N=5e5+5,M=1e9+7;
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
ll h[N];
inline int lowbit(int x)
{
    return x&(-x);
}
ll sum(int x){
    ll ans=0;
    for(int i=x;i>=1;i-=lowbit(i))ans=(ans+h[i])%M;
    return ans;
}
void add(int x,ll y)
{
    for(int i=x;i<=N;i+=lowbit(i))
        h[i]=(h[i]+y)%M;
}
ll a[N];
ll b[N];
ll n,t;
void discrete(){
    sort(b+1,b+n+1);
    t=unique(b+1,b+n+1)-(b+1);
}
int id(ll x){
    return lower_bound(b+1,b+t+1,x)-b;
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a[i];b[i]=a[i];
    }
    discrete();
    ll ans=0;
    for(int i=1;i<=n;i++){
        ans=(ans+(ll)i*(n-i+1)%M*a[i]%M)%M;
    }
    for(int i=1;i<=n;i++){
        int x=id(a[i]);
        ans=(ans+(ll)a[i]%M*sum(x)%M*(n-i+1)%M)%M;
        add(x,i);
    }
    memset(h,0,sizeof(h));
    for(int i=n;i>=1;i--){
        int x=id(a[i]);
        ans=(ans+(ll)a[i]%M*sum(x)%M*i%M)%M;
        add(x,n-i+1);
    }
    cout<<ans%M<<endl;
}
```