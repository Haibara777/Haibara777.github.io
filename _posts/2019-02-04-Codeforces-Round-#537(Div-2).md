---
layout: post
title:  "Codeforces Round #537(Div 2)"
date:   2019-02-04
desc: "Codeforces Round #537(Div 2)"
keywords: "Codeforces"
categories: [Acm]
tags: [Codeforces]
icon: icon-html
---


>写在前面
>
>自闭了吗？
>
>自闭了
>
>掉分了吗
>
>掉分了
>
>就算自闭到怀疑人生，还是要补题啊


艹艹艹艹艹艹艹艹艹艹艹艹

我真的怀疑人生了啊！！！！！！！！！！！！！

## A Superhero Transformation

没什么好说的

上代码

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1005;
char a[N],b[N];
bool check(char x)
{
    if(x=='a'||x=='e'||x=='i'||x=='o'||x=='u')return true;
    return false;
}
int main()
{
    scanf("%s%s",a,b);
    if(strlen(a)!=strlen(b)){
        cout<<"No"<<endl;
        return 0;
    }
    for(int i=0;i<(int)strlen(a);i++){
        if(check(a[i])&&!check(b[i])){
            cout<<"No"<<endl;
            return 0;
        }
        if(check(b[i])&&!check(a[i])){
            cout<<"No"<<endl;
            return 0;
        }
    }
    cout<<"Yes"<<endl;
}
```

## B Average Superhero Gang Power

哎，又被数据坑了，测试数据是无法涵盖所有情况，很多情况需要通过经验判断，这个题如果n大于m，去掉前面所有的绝对是最好的选择，这个题会很简单

但是。。。。

n还有可能小于m啊。。。。。

然后就是套路了，先认为一个都不删，然后再从小到大一个个删去，操作次数是 **n-1** 和 **m** 之间的最小值，然后每次删去总共增加的值为 **m-i** ，但是要小于等于 **(n-i)*k** 

一开始没能发现的是，删去的时候直接把加上的值加到总和上就可以，不许要在意加到了哪个值上，所以。。。。

一开始虽然想到了正解但是没有做出来，然后又傻逼得没能想到 **n>m** ，所以这次杯具完全是我自己的锅，我会认真检讨

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e6+1000;
ll a[N];
int main()
{
    ll n,m,k;
    cin>>n>>k>>m;
    ll sum=0;
    for(ll i=1;i<=n;i++){
        cin>>a[i];
        sum+=a[i];
    }
    ll s=min(n*k,m);
    double ans=(double)((double)(sum+s)/(double)n);
    sort(a+1,a+n+1);
    for(ll i=1;i<=min(n-1,m);i++)
    {
        ll h=min(m-i,(n-i)*k);
        sum-=a[i];
        ans=max(ans,(double)(sum+h)/(double)(n-i));
    }
    printf("%.20lf\n",ans);
}

```

## C Creative Snap

这个题学长说是分治，但是我不知道分治具体是什么东西。。。。

我一开始是想直接递归深搜就好，虽然是$$O({2}^{n})$$复杂度，但是其实可以剪枝，没有英雄在的区间直接消掉就好，就不用继续递归了，但是如何确定区间里有几个英雄对我来说是个难点，原本是想二分来查。。。

但是。。。

心态被B题搞崩了。。。

还好找到了跟我思路差不多的代码，我是真的忘了还有lower_bound和upper_bound了。。。。

先把数组排序，想找 **( l , r )** 中英雄的个数，因为 **l** 和 **r** 都是2的正整数幂，所以找第一个大于等于 **l** 的数的位置，和第一个大于 **r** 的数的位置，相减就是在区间里的英雄的个数

然后递归搜索就行了，每次比较，取拆或不拆中较小的那个

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=1e5+5;
ll a[N];
ll k,n,A,B;
ll dfs(int l,int r)
{
    ll t=upper_bound(a+1,a+k+1,r)-lower_bound(a+1,a+k+1,l);
    if(t==0){
        return A;
    }
    if(l==r)return t*B;
    return min(t*B*(r-l+1),dfs(l,(l+r-1)>>1)+dfs(((l+r-1)>>1)+1,r));
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>n>>k>>A>>B;
    for(int i=1;i<=k;i++)cin>>a[i];
    sort(a+1,a+k+1);
    ll ans=dfs(1,1<<n);
    cout<<ans<<endl;
}
```

## D Destroy the Colony

    unsolved

## E Tree

    unsolved


>过年啦，过年了
>
>woc又过年。。。。
>
>过年好烦。。。。