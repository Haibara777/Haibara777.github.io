---
layout: post
title:  "Codeforces Round #542 [Alex Lopashev Thanks-Round] (Div. 2)"
date:   2019-03-02
desc: "Codeforces Round #542 [Alex Lopashev Thanks-Round] (Div. 2)"
keywords: "Codeforces"
categories: [Acm]
tags: [Codeforces]
icon: icon-html
---

[比赛链接](http://codeforces.com/contest/1130)

>哟吼，打cf以来首次ak，虽然是赛后。。。难得遇上我全能做出来的一套题，但是打比赛的时候还是很坎坷，不仅又被long long坑了一次，被度错题坑了一次，还被邻接表数组开得不够大坑了两次。。。算起来耽误半个多小时（不算读错题的话）。。。可惜
>
>值得一提的是寒假快要结束了，应该只剩下明天一天，现在正处在回青岛的路上。据不完全统计，我这个寒假差不多写了112个程序，而且还在增加，真的不完全，并没有算上camp时写的那些
>
>嘛，上大学后的第一个假期就这么稀里糊涂的过去了，一直处在咕咕咕中或咕咕咕的路上，acm没怎么练，csapp也没怎么动，不过还好有camp在，如果不去参加camp，估计我这个寒假就要起飞了
>
>车上无聊，把这套题补了补，其实一周前就该补完的。。。算了，寒假过去就过去了，关键是下学期啊，不能再跟上学期一样咸鱼了，去尽量努力吧，做到自己最好的就足够了

## [A. Be Positive](http://codeforces.com/contest/1130/problem/A)

乍一看很牛逼，仔细一想就是个傻逼题，问除以多少后，数列里正数的个数大于数列长度的一半

虽然说明了要浮点数计算，其实问题不大，因为就算是整数运算，正数和负数的个数也不会改变，所以就变成了只输出1，-1，0的问题，如过正数和负数的个数都不大于一半，就输出0

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
int main()
{
    ios::sync_with_stdio(false);
    int n;
    int a=0,b=0;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        int k;
        cin>>k;
        if(k>0)a++;
        if(k<0)b++;
    }
    int mid=n/2;
    if(n%2==1)mid++;
    if(a>=mid){
        cout<<"1"<<endl;
    }
    else if(b>=mid){
        cout<<"-1"<<endl;
    }
    else if(a<mid&&b<mid){
        cout<<"0"<<endl;
    }
}
```

## [B. Two Cakes](http://codeforces.com/contest/1130/problem/B)

。。。竟然遇上了camp原题，不过这题比camp的简单多了，这个题是在一维上，camp的是在二维上，那个需要比较曼哈顿距离，当时差点被绕死

不过还是被long long坑了。。。。艹

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
const int N=1e5+10,M=1e5+3;
int a[N][3];
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n*2;i++)
    {
        int k;
        cin>>k;
        if(a[k][1]==0)a[k][1]=i;
        else a[k][2]=i;
    }
    ll ans=a[1][1]+a[1][2]-2;
    for(int i=2;i<=n;i++)
    {
        ans+=(ll)min(abs(a[i][1]-a[i-1][1])+abs(a[i][2]-a[i-1][2]),abs(a[i][1]-a[i-1][2])+abs(a[i][2]-a[i-1][1]));
    }
    cout<<ans<<endl;
}
```

## [C. Connect](http://codeforces.com/contest/1130/problem/C)

我又读错题了。。。硬生生把难度提高了一个档次

原来只能建一条隧道，我还以为是随便建，这样就白白浪费时间去写了一个最短路。。。其实只用并查集就好了。。。

原代码是在最短路的基础上改的，比较乱，我又写了一个清楚点的

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
const int N=50+10,M=1e5+3;
const double Pi=acos(-1.0);
char a[N][N];
int fa[N*N];
int n;
int dirx[5]={0,1,-1,0,0};
int diry[5]={0,0,0,1,-1};
int get(int x)
{
    if(fa[x]==x)return x;
    return fa[x]=get(fa[x]);
}
int id(int x,int y){
    return (x-1)*n+y;
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>n;
    int sx,sy,ex,ey;
    cin>>sx>>sy>>ex>>ey;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
            cin>>a[i][j];
    for(int i=1;i<=n*n;i++)fa[i]=i;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
        {
            if(a[i][j]=='1')continue;
            for(int k=1;k<=4;k++)
            {
                int x1=i+dirx[k];
                int y1=j+diry[k];
                if(x1<1||x1>n||y1<1||y1>n)continue;
                if(a[x1][y1]=='1')continue;
                if(get(id(x1,y1))==get(id(i,j)))continue;
                fa[get(id(i,j))]=get(id(x1,y1));
            }
        }
    int s=id(sx,sy),e=id(ex,ey);
    if(get(s)==get(e)){
        cout<<'0'<<endl;
        return 0;
    }
    int ans=1e8;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
        {
            int x=id(i,j);
            if(get(x)==get(s)){
                for(int i1=1;i1<=n;i1++)
                    for(int j1=1;j1<=n;j1++)
                    {
                        int y=id(i1,j1);
                        if(get(y)==get(e))ans=min(ans,(i-i1)*(i-i1)+(j-j1)*(j-j1));
                    }
            }
        }
    cout<<ans<<endl;
}

```

## [D. Toy Train](http://codeforces.com/contest/1130/problem/D2)

D题是两个题，D1是简化版本，所以直接做D2，D2能过D1就能过

依次算出每个车站从本站开始把本站的糖果全部送到目的地需要的时间，然后依次枚举每个车站，因为每次个车站不一定是从本车站出发的，所以每个车站都要加上从出发车站到此车站的距离，然后取最大值，就是需要的时间

需要注意的是，没有糖果的车站运送时间为0，而且不影响最后的结果，所以不需要考虑那个车站造成的影响

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
const int N=2e4+10,M=1e5+3;
const double Pi=acos(-1.0);
int a[N];
int p[N];
int w[N];
int main()
{
    ios::sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=m;i++)
    {
        int x,y;
        cin>>x>>y;
        if(p[x]){
            a[x]=min(a[x],(y-x+n)%n);
        }
        else a[x]=(y-x+n)%n;
        p[x]++;
    }
    for(int i=1;i<=n;i++)
        if(p[i])w[i]=(p[i]-1)*n+a[i];
    for(int i=1;i<=n;i++)
    {
        int ans=0;
        int t=i;
        for(int j=1;j<=n;j++)
        {
            if(p[t])ans=max(ans,w[t]+(t-i+n)%n);
            t++;
            if(t==n+1)t=1;
        }
        cout<<ans<<((i==n)?'\n':' ');
    }
}
```

## [E. Wrong Answer](http://codeforces.com/contest/1130/problem/E)

。。。当时最后的时间都用来想这个题了，虽然知道是构造，但是没想出来怎样构造

其实主要是当时没有想到第一个非零数是负数这种情况，知道后这道题就好解了

根据我当时的想法，是2000个数全部输出，然后前1998个数都是0，然后一个负数一个正数，假设这个负数是 **-a** 正数是 **b** ，于是需要满足的条件就是 **(b-a)\*2000-b==k** 即 **1999\*(b-a)==k+a** ，于是枚举 **k+a** 找到一个可以整除1999的就行了，枚举 **k** 后面2000个数是一定能找到的

因为 **k=1999\*b-2000\*a** ，a，b的最大值是1e6，所以这么构造时 **k** 的值是可以达到1e9的，因此不存在 -1 的情况

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
const int N=1e5+10,M=1e5+3;
int main()
{
    ios::sync_with_stdio(false);
    ll k;
    cin>>k;
    int x,y;
    for(int i=0;i<=2000;i++)
    {
        if((k+i)%1999==0){
            x=i;
            y=(k+i)/1999;
            break;
        }
    }
    cout<<2000<<endl;
    for(int i=1;i<=1998;i++)
        cout<<0<<' ';
    cout<<-x<<' '<<x+y<<endl;
}
```