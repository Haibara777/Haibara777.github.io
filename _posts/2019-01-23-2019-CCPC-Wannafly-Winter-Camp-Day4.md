---
layout: post
title:  "2019 CCPC Wannafly Winter Camp Day4"
date:   2019-01-23
desc: "2019 CCPC Wannafly Winter Camp"
keywords: "Camp"
categories: [Acm]
tags: [2019 CCPC Wannafly Winter Camp]
icon: icon-html
---

______

今天还是有点做题体验的，毕竟据说wls放了七道签到题，但是作为菜鸡的我们还是只做出了三道题，不过最起码没有前几天那么自闭了嘛。
>今天晚上出了点小插曲，因为晚上太多人没来上课，可能加上重感冒和舟车劳顿，wls自爆了，但是我感觉wls除了有些生气以外，也是想逼那些翘课的同学接着去上课，不能辜负wls的良苦用心啊，我们也要坚持下去。

## A 夺宝奇兵

看起来牛逼坏了的题。

其实，就是个贪心，你甚至可以在线操作，但是我懒啊哈哈哈哈哈哈哈哈哈。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=100005;
struct node{
    int x,y;
}a[N][3];
int d(int m,int n,int p1,int p2)
{
    return abs(a[m][p1].x-a[n][p2].x)+abs(a[m][p1].y-a[n][p2].y);
}
int main()
{
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i][1].x>>a[i][1].y>>a[i][2].x>>a[i][2].y;
    }
    ll ans=0;
    for(int i=2;i<=n;i++)
    {
        ans+=(ll)min(d(i-1,i,1,1)+d(i-1,i,2,2),d(i-1,i,1,2)+d(i-1,i,2,1));
    }
    ans+=(ll)d(n,n,1,2);
    cout<<ans<<endl;
}
```

## B 象象象棋

按题意模拟，标程500行。

 *“茶余饭后写一写”*       ---wls

## C 最小边覆盖

计算子集 **S** 组成的图中所有点的度数，如果存在孤立的点或存在两个度数为2的点相连，则该子集 **S** 不是最小边覆盖。

## D 欧拉回路

分两类情况，**m == 2\|\|n == 2** 和其他。

## E 黄金矿工

    unsolved    

## F 小小马

1.马走一步一定从黑走到白或者从白走到黑。
2.棋盘大于等于 3$$\times$$4  的网格一定是两两联通。
可以从这里直接搜索，或者：
3.对于大于 3$$\times$$4 的棋盘，求起点到终点的曼哈顿距离，若为偶数，则不合法，若为奇数，则合法。
4.然后再讨论一波 **n = 2** 或者 **m = 2** 和 **n = 2** 且 **m = 2**

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=100005;
int main()
{
    ios::sync_with_stdio(false);
    int n,m;
    int sx,sy,ex,ey;
    cin>>n>>m;
    cin>>sx>>sy>>ex>>ey;
    if(n==3&&m==3&&ex==2&&ey==2){
            cout<<"No"<<endl;
            return 0;
    }
    if(n==2&&m==2){
        cout<<"No"<<endl;
        return 0;
    }
    if(m==2){
        if(sy==ey){
            cout<<"No"<<endl;
        }
        else{
            if(abs(sx-ex)%4==2){
                cout<<"Yes"<<endl;
            }
            else{
                cout<<"No"<<endl;
            }
        }
        return 0;
    }
    if(n==2){
        if(sx==ex){
            cout<<"No"<<endl;
        }
        else{
            if(abs(sy-ey)%4==2){
                cout<<"Yes"<<endl;
            }
            else{
                cout<<"No"<<endl;
            }
        }
        return 0;
    }
    int d=abs(sx-ex)+abs(sy-ey);
    if(d%2==0){
        cout<<"No"<<endl;
    }
    else cout<<"Yes"<<endl;
}
```
## G 置置置换

f[i][j]表示前i个位置，在剩下的相对大小关系属于第j位
emmmm。。。我不知道自己听了什么，反正没听懂。

## H 命命命运

概率dp，就听懂一句 *“第i个人在第j轮第一次买到地k”* ，嘤嘤嘤。

## I 咆咆咆哮

div1：$$n\leq100000$$三分，写出收益函数，证明是凸型的。（但是我不会三分啊，啊哈哈哈哈哈哈哈）

所以我只会做div2的啊，一开始看到这个题非常激动，我以为是昨天大奶牛讲的题，但是仔细一看发现并不是，所以我就用了一些排序啊，dp啊这些奇奇怪怪的方法，然后就自闭了。

后来，90n大佬想起了大奶牛的做法，然后，我突然就想起来就算不是一样的题，方法可能是通用的啊，然后我就用之前wa掉的$$O(n^{2})$$的算法改造了一下，竟然ac了哈哈哈。

其实思路很简单啊，假设所有的卡牌都召唤生物，算出一个 **ans** 值来表示总攻击力，然后从所有卡牌中，找将生物换成buff后，对攻击力贡献最大的那个，枚举 **n** 次，只要最大贡献小于0就停止。

需要注意的是因为每次的最大贡献受前面替换卡牌的操作的影响，所以需要一个 **pre** 变量记录当前获得所有buff的总攻击力值。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1005;
struct node{
    int a,b;
}s[N];
int v[N]={0};
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>s[i].a>>s[i].b;
    }
    ll ans=0;
    for(int i=1;i<=n;i++)
    {
        ans+=(ll)s[i].a;
    }
    int pre=0;
    for(int i=1;i<=n;i++)
    {
        int maxx=-1e8,p;
        for(int j=1;j<=n;j++)
        {
            if(v[j])continue;
            int h=s[j].b*(n-i)-s[j].a-pre;
            if(h>maxx){
                maxx=h;
                p=j;
            }
        }
        if(maxx<=0)continue;
        v[p]=1;
        ans+=maxx;
        pre+=s[p].b;
    }
    cout<<ans<<endl;
}
```

## J 跑跑跑路

对于一个多元函数，最值在每一个变量的偏导都是0的地方。
或者~~梯度下降，改一下参数，或共轭梯度下降~~降维打击wls。

## K 两条路径

 *“div2你想怎么暴力就怎么暴力。”*    ---wls原话


