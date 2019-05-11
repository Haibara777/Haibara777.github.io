---
layout: post
title:  "2019 CCPC Wannafly Winter Camp Day1"
date:   2019-01-20
desc: "2019 CCPC Wannafly Winter Camp"
keywords: "Camp"
categories: [Acm]
tags: [2019 CCPC Wannafly Winter Camp]
icon: icon-html
---


______

 今天，我知道了什么是绝望
>请组织放心，我一定会补题的

## A 机器人

分类讨论，据wls说有6种情况，嘤嘤嘤，等我肝好了再慢慢写吧。

## B 吃豆豆

    我们几个菜鸡今天差点爆零，在这里容我再吹一波于昊大佬，抱着
    大佬的大腿，我们了a这唯一一道题。

这道题乍一看不是dp就是搜索，但是走过的路还可以走，所以搜索是行不通了，只能dp，于昊大佬的思路是 i , j , t dp三维，t就是当前时间，dp[i][j][t]就是第t秒( i , j )处最多获得的糖果数，需要注意的就是需要一个v数组来记录第t秒( i , j )是否到达过。

这道题div1版本的糖果数是1e18，因此要每2520秒循环一次，提前dp出一次循环中可拿到的最大糖果数k，然后将糖果数模k，再dp剩下的，但是因为我水平过低，不知道该如何dp，因此div1版本我真的做不出来。

```c++
#include<bits/stdc++.h>
using namespace std;
const int N=10005;
int a[20][20];
int dp[20][20][12005];
int dx[5]={0,0,1,0,-1};
int dy[5]={0,1,0,-1,0};
int v[20][20][12005]={0};
int main()
{
    ios::sync_with_stdio(false);
    int n,m,C;
    cin>>n>>m>>C;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            cin>>a[i][j];
    int sx,sy,ex,ey;
    cin>>sx>>sy>>ex>>ey;
    v[sx][sy][0]=1;
    for(int t=1;t<=C*20;t++)
    {
        for(int i=1;i<=n;i++)
            for(int j=1;j<=m;j++){
                for(int k=0;k<=4;k++)
                {
                    int x1=i+dx[k],y1=j+dy[k];
                    if(v[x1][y1][t-1]==0)continue;
                    v[i][j][t]=1;
                    dp[i][j][t]=max(dp[i][j][t],dp[x1][y1][t-1]);
                }
                if(t%a[i][j]==0&&v[i][j][t])dp[i][j][t]++;
                if(dp[ex][ey][t]>=C){
                    cout<<t<<endl;
                    return 0;
                }
            }
    }
}
```

## C 拆拆拆数

c题还是挣扎了很久的，但是并没有什么卵用，显而易见的是只要是题目给的数据，就一定有解，且n<=2，当a，b互质时n=1，否则n=2，但是难以证明，而且拆数的时候还是一筹莫展

然而之后看了别人的代码发现，只需要枚举前几个素数就行了。。。。
坑啊！！！！！！
```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5,M=998244353;
const ll p[10]={0,2,3,5,7,11,13,17};
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    ll a,b;
    while(t--&&cin>>a>>b){
        if(__gcd(a,b)==1){
            cout<<'1'<<endl;
            cout<<a<<' '<<b<<endl;
            continue;
        }
        cout<<'2'<<endl;
        int flag=0;
        for(int i=1;i<=7;i++){
            for(int j=1;j<=7;j++)
            {
                if(p[i]!=p[j]&&__gcd(a-p[i],b-p[j])==1){
                    flag=1;
                    cout<<p[i]<<' '<<p[j]<<endl;
                    cout<<a-p[i]<<' '<<b-p[j]<<endl;
                    break;
                }
            }
            if(flag)break;
        }
    }
}
```
## D 超难的数学题

    unsolved

## E 流流流动
##### 更新于2019-1-29
_______
懵逼啊，这个题最终会生成树，可以用并查集记录一下，把不是一棵树的几个树连到点0，合成一棵树，然后树形dp。。。。。
```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=10005,M=998244353;
int head[N],Next[N],ver[N],tot=0;
void add(int x,int y){
    tot++;
    ver[tot]=y;
    Next[tot]=head[x];
    head[x]=tot;
}
int f[105],d[105];
int dp[105][2];
int v[105];
int fa[105];
int get(int x)
{
    if(fa[x]==x)return x;
    return fa[x]=get(fa[x]);
}
void dfs(int p)
{
    dp[p][0]=0;
    dp[p][1]=f[p];
    for(int i=head[p];i;i=Next[i])
    {
        int y=ver[i];
        if(v[y])continue;
        v[y]=1;
        dfs(y);
        dp[p][0]+=max(dp[y][0],dp[y][1]);
        dp[p][1]+=max(dp[y][0],dp[y][1]-d[min(p,y)]);
    }
}
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)cin>>f[i];
    for(int i=1;i<=n;i++)cin>>d[i];
    for(int i=1;i<=n;i++)fa[i]=i;
    for(int i=2;i<=n;i++)
    {
        if(i%2==1){
            if(3*i+1<=n){
                add(i,3*i+1);
                add(3*i+1,i);
                if(get(i)!=get(3*i+1))fa[get(i)]=get(3*i+1);
            }
        }
        else {
            add(i,i/2);
            add(i/2,i);
            if(get(i)!=get(i/2))fa[get(i)]=get(i/2);
        }
    }
    for(int i=1;i<=n;i++)
    {
        if(fa[i]==i){
            add(get(i),0);
            add(0,get(i));
        }
    }
    v[0]=1;
    dfs(0);
    cout<<max(dp[0][1],dp[0][0])<<endl;
}
```

## F 爬爬爬山
##### 更新于2019-1-25
____
这个题很简单啊啊啊啊啊啊啊！！！！！！！

其实就是单源最短路，插入路径的时候，走向不能到达的山的时候需要削山头。

然后需要注意的就是没有负边权，对这也要注意，因为我学到一句话， **“没有负边权的题都是在卡spfa”** ，所以我spfa光荣被t，需要用堆优化的Dijkstra，或者也用堆优化一下spfa（可能是数据的问题，优化过的spfa之比dijkstra快一点）。

值得一提的是，spfa的堆优化代码只比dijkstra多一点点。
```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=4e5+5;
int a[N/2];
int Next[N],head[N]={0},ver[N],tot=0;
ll edge[N];
ll dis[N];
int v[N]={0};
void add(int x,int y,ll v)
{
    tot++;
    ver[tot]=y;
    edge[tot]=v;
    Next[tot]=head[x];
    head[x]=tot;
}
struct node{
    int p;
    ll v;
    friend bool operator < (node n,node m){
        return n.v>m.v;
    }
};
int main()
{
    ios::sync_with_stdio(false);
    memset(dis,0x3f,sizeof(dis));
    int n,m,k;
    cin>>n>>m>>k;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    int h=a[1]+k;
    for(int i=1;i<=m;i++)
    {
        int x,y;
        ll z;
        cin>>x>>y>>z;
        if(a[y]>h){
            add(x,y,z+(ll)(a[y]-h)*(a[y]-h));
        }
        else add(x,y,z);
        if(a[x]>h){
            add(y,x,z+(ll)(a[x]-h)*(a[x]-h));
        }
        else add(y,x,z);
    }
    priority_queue<node> Q;
    node c;
    c.p=1;c.v=0;
    Q.push(c);
    dis[1]=0;
    while(!Q.empty())
    {
        c=Q.top();
        Q.pop();
        int x=c.p;
        //v[x]=0;   没有这里就是dijkstra，加上就是spfa
        for(int i=head[x];i;i=Next[i])
        {
            int y=ver[i];
            ll vv=(ll)edge[i];
            if(dis[y]>dis[x]+vv)
            {
                dis[y]=dis[x]+vv;
                if(v[y])continue;
                node d;
                d.p=y;d.v=dis[y];
                Q.push(d);
                v[y]=1;
            }
        }
    }
    cout<<dis[n]<<endl;
}
```

## G 双重矩阵

    unsolved

## H 我爱割葱

    unsolved

## I 起起落落
##### 更新于2019-1-31
_____
一开始看了题解还是懵逼的，啃了半天代码后总算是有了些理解，就把代码些出来了（其实跟默写没什么区别emmm）

其实这道题整体思路还是dp，只不过用树状数组进行区间查询

对于一个点 **p1** 来说，它后面有一类这样的点：

1. 点的值大于点 **p1** 的值，假设 **p3** 为这样的点
2. 对 **p3** 来说，存在一个 **p2** ，且 **p2** 的值小于 **p3** 的值

满足以上条件的 **p3** 都是这一类点

这一类点都会构成答案，而且答案的值取决于它后面有几个 **p3** 以及前面有几个 **p2** 

因此，我们用 **w** 数组记录当前面出现一个大于这个点的点时，这个点做出的贡献，而前面每出现一个小于这个点的点，这个点做出的贡献都会翻倍

因为前面会对后面产生影响，因此，我们从后往前动态规划

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=2e3+5,M=1e9+7;
int a[N];
int c[N]={0};
int n;
int q[N][N]={0};
int w[N];
int lowbit(int x){
    return x&(-x);
}
int sum(int x){
    int ans=0;
    for(int i=x;i>=1;i-=lowbit(i))ans=(ans+c[i])%M;
    return ans;
}
void add(int x,int y)
{
    for(int i=x;i<=N;i+=lowbit(i))
        c[i]=(c[i]+y)%M;
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>n;
    for(int i=1;i<=n;i++)cin>>a[i];
    for(int i=1;i<=n;i++)
        for(int j=i+1;j<=n;j++)
        {
            if(a[j]>a[i]){
                q[i][0]++;
                q[i][q[i][0]]=j;
            }
        }
    int ans=0;
    for(int i=n;i>=1;i--)
    {
        w[i]=sum(a[i]-1);
        ans+=w[i];
        ans%=M;
        w[i]++;
        for(int j=1;j<=q[i][0];j++)
        {
            add(a[q[i][j]],w[q[i][j]]);
        }
    }
    cout<<ans%M<<endl;
}
```
    
## J 夺宝奇兵
##### 更新于2019-1-28
____

虽然做出来后还是比较懵逼，但是起码理解了一些

枚举 **i** ， **i** 表示wls最终手里得到的宝物数，然而wls如果想获得这些宝物，就需要所有人的宝物数都小于等于 **i-1** ，于是，他需要将宝物数比  **i** 多的人买到宝物剩下 **i-1** ，如果手中的宝物数还是不够的话，就从剩下的宝物中买最便宜的

因为 **i** 可能的取值只有 **1~m** ，枚举所有的后就没有其他的情况了，所以所有枚举得到的值就是答案所有的可能了，从中取最小的就可以了

>用优先队列做竟然没有被t，emmmmmmmm

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1005;
struct node {
    int id;
    ll v;
    friend bool operator < (const node &a,const node &b){
        return a.v>b.v;
    }
}a[N];
int v[N]={0};
int c[N];
int h[N]={0};
int main()
{
    priority_queue<node> Q[N],qq;
    ios::sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=m;i++){
        cin>>a[i].v>>c[i];
        h[c[i]]++;
        a[i].id=i;
    }
    ll ans=1e16;
    for(int i=1;i<=m;i++){
        ll sum=0;
        int num=0;
        while(!qq.empty())qq.pop();
        for(int j=1;j<=n;j++){
            while(!Q[j].empty())Q[j].pop();
        }
        for(int j=1;j<=m;j++){
            Q[c[j]].push(a[j]);
            qq.push(a[j]);
        }
        memset(v,0,sizeof(v));
        for(int j=1;j<=n;j++)
            if(h[j]>=i)
                for(int k=1;k<=h[j]-i+1;k++){
                    node c=Q[j].top();
                    sum+=c.v;num++;
                    v[c.id]=1;
                    Q[j].pop();
                }
        for(int j=1;j<=m&&num<i;j++){
            node c=qq.top();
            qq.pop();
            if(v[c.id])continue;
            sum+=c.v;
            num++;
        }
        ans=min(ans,sum);
    }
    cout<<ans<<endl;
}
```

## K 星球大战

    unsolved

