---
layout: post
title:  "Codedorces 2017 JUST Programming Contest 4.0"
date:   2019-02-09
desc: "Codedorces 2017 JUST Programming Contest 4.0"
keywords: "Codedorces"
categories: [Acm]
tags: [Codedorces]
icon: icon-html
---


>这真的不是我今天想补的那一套题。。。。
>
>奥对。。又咕CSAPP了。。。
>
>还有就是这套题好多个只能用scanf和printf的啊啊啊啊啊！！！！！！坑得我G题超时了半天，心态炸裂，只能被大佬血虐。。。

## A. Subarrays Beauty

这个题把样例写出来找一波规律就好了。。。二进制的每一位之间的运算互不影响，所以对每一位计算一遍就行了，如果碰到零就要重新计算
```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e6+5;
int a[N];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n;
        scanf("%d",&n);
        for(int i=1;i<=n;i++)
        {
            scanf("%d",&a[i]);
        }
        int flag=1;
        int k=0;
        ll ans=0;
        int num;
        while(flag){
            flag=0;
            int x=(1<<k);
            num=1;
            for(int i=1;i<=n;i++)
            {
                if(a[i]!=0)flag=1;
                if(a[i]&1)
                {
                    ans+=(ll)x*num;
                    num++;
                }
                else {
                    num=1;
                }
                a[i]>>=1;
            }
            k++;
        }
        printf("%I64d\n",ans);
    }
}
```

## B. Array Reconstructing

前后扫两遍就行了，唯一在状态的题。。。下一个题开始就崩盘了。。。
```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1005;
ll a[N];
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--){
        int n;
        ll m;
        cin>>n>>m;
        for(int i=1;i<=n;i++)
        {
            cin>>a[i];
        }
        for(int i=2;i<=n;i++)
        {
            if(a[i]==-1&&a[i-1]!=-1){
                a[i]=(a[i-1]+1+m)%m;
            }
        }
        for(int i=n-1;i>=1;i--)
        {
            if(a[i]==-1&&a[i+1]!=-1){
                a[i]=(a[i+1]-1+m)%m;
            }
        }
        cout<<a[1];
        for(int i=2;i<=n;i++)
            cout<<' '<<a[i];
        cout<<endl;
    }
}
```

## C. Large Summation

思路很好想，二分查找与当前元素和小于1e9+7的最大值，和它与所有值的最大值相加模1e9+7相比较，取较大的那一个，还有就是特判相加的两个值不能是同一个位置的值。。。

但是好难写啊，调了半天总算过了。。。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5,M=1e9+7;
struct node{
    int v,p;
}a[N];
int c[N],d[N];
bool cmp(node n,node m)
{
    return n.v<m.v;
}
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n;
        scanf("%d",&n);
        for(int i=1;i<=n;i++)
        {
            scanf("%d",&c[i]);
            a[i].v=c[i];
            a[i].p=i;
        }
        sort(a+1,a+n+1,cmp);
        for(int i=1;i<=n;i++)
            d[i]=a[i].v;
        for(int i=1;i<=n;i++)
        {
            int h=M-c[i];
            int k=lower_bound(d+1,d+n+1,h)-d-1;
            int ans1,ans2;
            while(d[k]+c[i]>M)k--;
            if(a[k].p==i)ans1=c[i]+d[--k];
            else ans1=c[i]+d[k];
            if(k<=0)ans1=0;
            if(i!=a[n].p)ans2=(c[i]+a[n].v)%M;
            else ans2=(c[i]+a[n-1].v)%M;
            c[i]=max(ans1,ans2);
            continue;
        }
        printf("%d",c[1]%M);
        for(int i=2;i<=n;i++)
        {
            printf(" %d",c[i]%M);
        }
        printf("\n");
    }
}

```

## D. Counting Test

这个题我开了波脑洞，预处理了一波，想不到还真过了。。。心态炸裂啊，心态已经炸裂了。。。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e4+5;
char a[N];
int dp[N][30];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n,k;
        scanf("%d%d",&n,&k);
        scanf("%s",a+1);
        memset(dp,0,sizeof(dp));
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=26;j++){
                dp[i][j]=dp[i-1][j];
                if(j==a[i]-'a'+1)dp[i][j]++;
            }
        }
        while(k--)
        {
            int l,r;
            char w[2];
            scanf("%d%d%s",&l,&r,w);
            int lll=l/n;
            if(n*lll==l)lll--;
            int rr=r/n;
            ll ans=0;
            ans+=(ll)(rr-lll)*dp[n][w[0]-'a'+1];
            int h=(l-1+n)%n;
            if(h>=0)ans-=(ll)dp[h][w[0]-'a'+1];
            h=(r%n);
            ans+=(ll)dp[h][w[0]-'a'+1];
            printf("%I64d\n",ans);
        }
    }
}
```

## E. Game of Dice
##### 更新于2019-2-11
____

今天把这个题的思路看了一下，meet in middle，就是把搜索的部分分成两部分，分别搜索，可以把这个题的时间复杂度从$${6}^{n}$$变成$$2\times{6}^{\frac{n}{2}}$$

于是，就不超时了

但是我想了半天还是没想出来该如何处理两个部分的搜索结果才能求出答案，然后上网查了一下，看明白了该如何处理

第一部分搜索结束记录乘积出现的次数，第二部分记录k/乘积在第一部分中出现的次数的总和，就是答案，当然要求一下逆元。。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5,M=1e9+7;
int a[15][10];
map<ll,int> m;
int n;
ll k;
ll qpow(ll x,int y)
{
    ll ans=1,k=x;
    while(y){
        if(y&1)ans=(ans*k)%M;
        k=(k*k)%M;
        y>>=1;
    }
    return ans;
}
ll ans=0;
void dfs(int l,int r,int flag,ll num)
{
    if(l==r){
        if(flag){
            m[num]++;
        }
        else {
            ans+=m[k*qpow(num,M-2)%M];
        }
    }
    else{
        for(int i=1;i<=6;i++)
            dfs(l+1,r,flag,num*(ll)a[l+1][i]%M);
    }
}
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d%I64d",&n,&k);
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=6;j++)
            {
                scanf("%d",&a[i][j]);
            }
        }
        ans=0;
        m.clear();
        dfs(0,n/2,1,1);
        dfs(n/2,n,0,1);
        printf("%I64d\n",ans);
    }
}
```

## F. Strings and Queries

    unsolved

## G. Magical Indices

心态炸裂题啊！！！上网一查发现要用scanf和printf。。不然超时。。。

从左到右每次记录当前最大值，大于等于前面最大值的标记1，从右到左记录当前最小值，小于等于后面最小值的标记1，最后两个标记都是1的位置就是要找的元素

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e6+5;
int a[N];
int l[N];
int r[N];
int main()
{
    int t;
    //ios::sync_with_stdio(false);
    //cin>>t;
    scanf("%d",&t);
    while(t--){
        int n;
        //cin>>n;
        scanf("%d",&n);
        for(int i=1;i<=n;i++)scanf("%d",&a[i]);
        int maxx=a[1],minn=a[n];
        for(int i=2;i<=n-1;i++)
        {
            if(a[i]>=maxx){
                maxx=a[i];
                l[i]=1;
            }
            else l[i]=0;
        }
        for(int i=n-1;i>=2;i--)
        {
            if(a[i]<=minn)
            {
                minn=a[i];
                r[i]=1;
            }
            else r[i]=0;
        }
        int ans=0;
        for(int i=2;i<=n-1;i++)
        {
            if(l[i]&&r[i])ans++;
        }
        //cout<<ans<<endl;
        printf("%d\n",ans);
    }
}
```

## H. Corrupted Images

。。。纯瞎搞，甚至读一遍就可以了，之前啊写了个巨傻逼的，现在想一想其实可以在线操作。。。
```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e6+5;
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        int n,m;
        cin>>n>>m;
        int tot=0;
        int fix=0;
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=m;j++){
                char c;
                cin>>c;
                if(c=='1'){
                    if(i!=1&&i!=n&&j!=1&&j!=m)tot++;
                }
                if(c=='0'){
                    if(i==1||j==1||i==n||j==m)fix++;
                }
            }
        }
        if(fix>tot){
            cout<<"-1"<<endl;
        }
        else {
            cout<<fix<<endl;
        }
    }
}
```

## I. The Crazy Jumper

从1到n走和从n到1走都是一样的，对于每一个位置只有可能从上一个同数字的元素和前面相邻的一个元素过来，于是可以dp，取每个位置的较小值

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=2e5+5,M=1e9+7;
int c[N];
int Next[N];
int dp[N];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n;
        scanf("%d",&n);
        memset(c,0,sizeof(c));
        for(int i=1;i<=n;i++)
        {
            int k;
            scanf("%d",&k);
            Next[i]=c[k];
            c[k]=i;
        }
        dp[1]=0;
        dp[0]=1e8;
        for(int i=2;i<=n;i++)
        {
            dp[i]=min(dp[i-1],dp[Next[i]])+1;
        }
        printf("%d\n",dp[n]);
    }
}
```
## J. The Hell Boy

啊，这个题，本来是没有思路的，但是经过我一系列的因式分解，偶然发现：

**a+b+c+a\*b+a\*c+b\*c+a\*b\*c=(a+b+a\*b)+c\*(a+b+a\*b)+c**

于是大胆的猜测这就是递推式。。

然后过了。。。

但是具体怎么证呢，我觉得可以用数学归纳法，但是没有具体去证。。。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5,M=1e9+7;
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n;
        scanf("%d",&n);
        ll ans;
        scanf("%I64d",&ans);
        for(int i=2;i<=n;i++){
            int k;
            scanf("%d",&k);
            ans=(ans*(k+1)%M+k%M)%M;
        }
        printf("%I64d\n",ans);
    }
}

```

## K. Palindromes Building

不会啊这个题，于是上网找了波题解，排列组合没学好。。。。

要想构成回文串，如果n是偶数，需要每个字母数量都是偶数，如果n是奇数，需要有且只有一个字母数量是奇数

如果能构成回文串，只需要考虑一半可能的组成就行了，所以就是排列组合，但是我不会啊23333333

所以被大佬血虐。。。还不给我讲题嘤嘤嘤嘤嘤嘤嘤嘤嘤

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=25;
char a[N];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n;
        scanf("%d",&n);
        scanf("%s",a+1);
        int s[30]={0};
        memset(s,0,sizeof(s));
        for(int i=1;i<=n;i++)
        {
            s[a[i]-'a'+1]++;
        }
        int flag=0;
        if(n%2==0)
        {
            for(int i=1;i<=26;i++)
            {
                if(s[i]%2==1){flag=1;break;}
            }
        }
        else 
        {
            int num=0;
            for(int i=1;i<=26;i++)
                if(s[i]%2==1)num++;
            if(num!=1)flag=1;
        }
        if(flag){
            printf("0\n");
            continue;
        }
        ll A1=1,A2=1;
        for(int i=1;i<=(n>>1);i++)
        {
            A1*=(ll)i;
        }
        for(int i=1;i<=26;i++)
        {
            if(s[i]!=0){
                for(int j=1;j<=(s[i]>>1);j++)
                    A2*=(ll)j;
            }
        }
        printf("%I64d\n",A1/A2);
    }
}
```

>以上代码都精简了一下头文件和其他不必要的东西，也不知道能不能过23333