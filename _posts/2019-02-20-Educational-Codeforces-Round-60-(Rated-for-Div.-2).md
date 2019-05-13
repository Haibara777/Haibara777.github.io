---
layout: post
title:  "Educational Codeforces Round 60 (Rated for Div. 2)"
date:   2019-02-20
desc: "Educational Codeforces Round 60 (Rated for Div. 2)"
keywords: "Codeforces"
categories: [Acm]
tags: [Codeforces]
icon: icon-html
---


>果然还是只能做出两题啊，这次c题d题对我来说太硬核了，e题是个交互题，一般不会太难。。。吧。。。有时间再去补吧

## [A. Best Subsegment](http://codeforces.com/contest/1117/problem/A)

不就是找最大的数连续出现的最大的次数嘛，一开始脑子卡了。。。写法有点跑偏，还好及时改成了一种简单的

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5,M=1e5+3;
ll a[N];
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    int ans=0;
    ll maxx=0;
    int num=0;
    for(int i=1;i<=n;i++){
        maxx=max(maxx,a[i]);
    }
    for(int i=1;i<=n;i++)
    {
        if(maxx==a[i])num++;
        else num=0;
        ans=max(ans,num);
    }
    cout<<ans<<endl;
}
```

## [B. Emotes](http://codeforces.com/contest/1117/problem/B)

就只有最大值（max1）和第二大值（max2）有用，然后就 **k** 个 **max1** ， **1** 个 **max2**一循环，找前 **n** 项和就行了

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=2e5+5,M=1e5+3;
int a[N];
int main()
{
    ios::sync_with_stdio(false);
    int n,m,k;
    cin>>n>>m>>k;
    for(int i=1;i<=n;i++)
        cin>>a[i];
    sort(a+1,a+n+1);
    int max1=a[n];
    int max2=a[n-1];
    ll ans=0;
    ll num1=(ll)max1*k+(ll)max2;
    ans+=(ll)(m/(k+1))*num1;
    ll num2=(ll)m%(k+1);
    ans+=num2*max1;
    cout<<ans<<endl;
}
```

## [C. Magic Ship](http://codeforces.com/contest/1117/problem/C)

我是真的没有想到这个题竟然可以二分，其实我一开始想的是只要能算出一个循环节走的路程，循环多次后直接暴搜找到到终点的时间，每一天根据风向和船行驶方向的不同，两点之间的曼哈顿距离会增加0或1或2

但是这么太难算了。。。

于是我去看了别人的代码，于是我就发现连风和船可以分开考虑都没有想到。。。

因为每天通过船的位移可以使两点之间的曼哈顿距离减少一，所以在只靠风力行驶的过程中，如果船与终点之间的曼哈顿距离小于等于行驶的天数，那么这些天就可以到达

所以只要求出循环节来就行，因为如果直接计算循环次数时，需要对曼哈顿距离计算时的绝对值进行讨论，比较复杂，而对于循环次数又可以很容易得判断是否可行，所以可以使用二分查找循环次数

因为就算每个循环节只有一天，所以循环1e10此后，能到终点的一定到了，到不了的只有不能到的，所以先假设循环次数为1e10，到不了的直接输出-1。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5,M=1e5+3;
string s;
ll a[N];
ll b[N];
ll x1,x2,yyy,y2;
ll n;
ll x=0,y=0;
bool check(ll k){
    ll yy=abs(y2-yyy-k*y);
    ll xx=abs(x2-x1-k*x);
    if(xx+yy<=k*n){
        return true;
    }
    else return false;
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>x1>>yyy>>x2>>y2>>n>>s;
    for(int i=0;i<n;i++){
        if(s[i]=='U')y++;
        if(s[i]=='D')y--;
        if(s[i]=='L')x--;
        if(s[i]=='R')x++;
        a[i+1]=x;b[i+1]=y;
        if(abs(y2-yyy-y)+abs(x2-x1-x)<=i+1){
            cout<<i+1<<endl;
            return 0;
        }
    }
    if(!check(1e10)){
        cout<<"-1"<<endl;
        return 0;
    }
    ll l=0,r=1e10;
    while(l<r){
        ll mid=l+((r-l)>>1);
        if(check(mid)){
            r=mid;
        }
        else l=mid+1;
    }
    l--;
    for(int i=1;i<=n;i++)
    {
        if(abs(y2-yyy-b[i]-l*y)+abs(x2-x1-a[i]-l*x)<=l*n+i){
            cout<<l*n+i<<endl;
            return 0;
        }
    }
}
```

## [D. Magic Gems](http://codeforces.com/contest/1117/problem/D)

为了这个题我去学了矩阵快速幂和矩阵加速递推，终于做出来了

其实这个题就是一个递推式，**m** 固定后，设 **f(x)** 为空间大小为 **x** 时的答案数，于是对于 **x-1** 的空间大小，如果空间大小加一，会出现两种情况：

1. 有一个普通宝石的最右端放在新出的空位上
2. 没有一个普通宝石放在最右端的空位上

两种情况的方案数分别时 **f(x-m)** 和 **f(x-1)** 于是递推式就来了。。

因为n最大1e18，所以需要矩阵加速递推，这玩意儿真的神奇，构造一个神奇的矩阵，就可以实现递推，又因为快速幂所以可以加速递推

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1005,M=1e9+7;
struct mat
{
    ll a[105][105];
    int n,m;
    void res(){
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
                a[i][j]=(i==j)?1:0;
    }
    void set(ll k){
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
                a[i][j]=k;
    }
};
mat mul(mat x,mat y)
{
    mat ans;
    ans.n=x.n,ans.m=y.m;
    ans.set(0);
    for(int i=1;i<=ans.n;i++)
        for(int j=1;j<=ans.m;j++)
            for(int k=1;k<=x.m;k++)
            {
                ans.a[i][j]+=x.a[i][k]*y.a[k][j]%M;
                ans.a[i][j]%=M;
            }
    return ans;
}
mat mqpow(mat x,ll y){
    mat h=x,ans=x;
    ans.res();
    while(y){
        if(y&1)ans=mul(ans,h);
        h=mul(h,h);
        y>>=1;
    }
    return ans;
}
int main()
{
    ios::sync_with_stdio(false);
    ll n,m;
    cin>>n>>m;
    if(n<m){
        cout<<1<<endl;
        return 0;
    }
    mat c;
    c.m=m,c.n=m;
    c.a[1][1]=1;c.a[1][m]=1;
    for(int i=1;i<m;i++)
    {
        c.a[i+1][i]=1;
    }
    mat ans=mqpow(c,n);
    cout<<ans.a[1][1]%M<<endl;
}
```

## [E. Decypher the String](http://codeforces.com/contest/1117/problem/E)
##### 更新于2019-2-23
____

交互题，其实不难，把每个字母编号，然后转换，就能找到原来的字符串

因为字母最多26个，只能询问三次，所以可以编号的最长长度是 **26\*26\*26+26\*26+26=18278** ，因为字符串最长1e4，所以足够了，而经过计算，最短可以用22进制编号

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
char ans[N];
int main()
{
    ios::sync_with_stdio(false);
    string s,s1,s2,s3,ss;
    cin>>s;
    int n=s.size();
    ss.clear();
    for(int i=0;i<n;i++)
        ss.push_back(char(i%22+'a'));
    cout<<"? "<<ss<<endl;
    cin>>s1;
    cout.flush();
    ss.clear();
    for(int i=0;i<n;i++)
        ss.push_back(char((i/22)%22+'a'));
    cout<<"? "<<ss<<endl;
    cin>>s2;
    cout.flush();
    ss.clear();
    for(int i=0;i<n;i++)
        ss.push_back(char(i/(22*22)+'a'));
    cout<<"? "<<ss<<endl;
    cin>>s3;
    cout.flush();
    ss.clear();
    for(int i=0;i<n;i++)
    {
        int x=(s1[i]-'a')+(s2[i]-'a')*22+(s3[i]-'a')*22*22;
        ans[x]=s[i];
    }
    cout<<"! ";
    for(int i=0;i<n;i++)
        cout<<ans[i];
    cout<<endl;
}
```

## [F. Crisp String](http://codeforces.com/contest/1117/problem/F)

    unsolved

## [G. Recursive Queries](http://codeforces.com/contest/1117/problem/G)

    unsolved