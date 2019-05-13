---
layout: post
title:  "Codeforces Round #539 (Div. 2)"
date:   2019-02-18
desc: "Codeforces Round #539 (Div. 2)"
keywords: "Codeforces"
categories: [Acm]
tags: [Codeforces]
icon: icon-html
---

>昨天，不，今天早上的状态估计是我有史以来最差的一次，又因为读错题，b题一直没做出来，加上心态爆炸，状态越来越差。。。
>
>没办法，还是太头铁了，前一天就睡了不到五个小时，一天都不在状态，晚上熬夜真的时很勉强了。。

## A. Sasha and His Trip

这个题也没多想，随便写了下交上去，竟然过了，然后我到了一百多名，估计是有史以来最高名次了（我当时在想之后会不会爆炸啊，果然爆了。。。）

明显的贪心，能尽量在前面加油就在前面加

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=4e4+5,M=1e9+7;
int main()
{
    ios::sync_with_stdio(false);
    int n,v;
    cin>>n>>v;
    int ans=0;
    if(v>=n-1){
        ans=n-1;
    }
    else {
        ans+=v;
        for(int i=2;i<=n-v;i++)
            ans+=i;
    }
    cout<<ans<<endl;
}
```

## B. Sasha and Magnetic Machines

我真的好像去死啊！！！！

题目是只能操作一次，我按可以操作无数次来做的，半天过不了，彻底自闭

然后看了别人代码恍然大悟。。。

艹。。。

之前代码随便一删就过了，因为之前用了优先队列，可以多次操作，比较麻烦，所以下面贴一份精简过的

直接枚举所有会使和减少的可能，如果这两个数都存在，就记录下来，取所有可能的最大值就行了。。。

我真的好像去死啊。。。。艹艹艹艹艹

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=4e4+5,M=1e9+7;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
int c[105];
int main()
{
    int n;n=read();
    int tot=0;
    for(int i=1;i<=n;i++)
    {
        int k=read();
        tot+=k;
        c[k]=1;
    }
    int maxx=0;
    for(int i=1;i<=100;i++)
    {
        for(int j=1;j<=100;j++)
            for(int k=2;k<=100;k++)
            {
                if(i%k==0&&j*k<=100){
                    int h=i+j-i/k-j*k;
                    if(h<=0)continue;
                    if(c[i]&&c[j])maxx=max(maxx,h);
                }
            }
    }
    printf("%d\n",tot-maxx);
}
```

## C. Sasha and a Bit of Relax

一开始的思路其实没毛病，异或同一个数两次后值不变，但是没有遇见过这种套路，所以没能想出来

在这个题中，对这个结论进行了扩展，异或同一个数两次就相当于异或0，即相同的数的异或值是0，所以如果是多个数的异或和为零，那么不管怎么异或，剩下的最后两个数一定相等

将所有的值不断异或，异或途中，如果同一个数出现两次，那么前面那个数的位置后面一直到后面那个数的位置之间所有的数的异或相当于没有操作，也就是说，这个区间的异或和为0，而且不管怎么分，两边的异或和都相等，所以如果可以等分的话这就是答案之一了

至于是否可以等分，就多开一维记录一下这个数所在位置的奇偶就行了

对了还有就是要开long long，我因为这个wa了好几发。。

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=5e6+5,M=1e9+7;
int dp[N][2];
int main()
{
    int n;
    ios::sync_with_stdio(false);
    cin>>n;
    ll sum=0;
    ll ans=0;
    dp[0][0]=1;
    for(int i=1;i<=n;i++)
    {
        ll x;
        cin>>x;
        sum^=x;
        ans+=dp[sum][(i+2)%2];
        dp[sum][(i+2)%2]++;
    }
    cout<<ans<<endl;
}
```

## D. Sasha and One More Name

刚开始想爆搜来着，dfs搜索两边是不是回文串，后来发现没那么麻烦，好像答案就只有1，2，Impossible。。。

于是就开始暴力枚举，但是很不幸，不太对

然后去开别人写的，仔细想想才发现，如果是奇数的回文串，只有一种情况是不可能，即除了中间那个字母，其余字母全部相同的情况，其他所有情况答案都是2

如果是偶数的回文串，情况比较复杂，只要不是所有的字母都相等，答案就存在，不是1就是2，所以这样剩下的才可以直接暴力枚举，看是否有只切一次就可以的情况，如果没有就切两次，切两次一定符合要求

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int N=5e6+5,M=1e9+7;
string s;
bool check(string s)
{
    for(int i=0,j=s.size()-1;i<=j;i++,j--)
    {
        if(s[i]!=s[j])return false;
    }
    return true;
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>s;
    int len=s.size();
    int flag=0;
    for(int i=1;i<len;i++)
    {
        if(s[i]!=s[0])flag++;
    }
    if(flag<=1){
        cout<<"Impossible"<<endl;
        return 0;
    }
    if(len%2==1){
        cout<<'2'<<endl;
        return 0;
    }
    else {
        for(int i=1;i<len;i++)
        {
            string s1(s.begin(),s.begin()+i);
            string s2(s.begin()+i,s.begin()+len);
            if(check(s2+s1)&&s2+s1!=s){
                cout<<'1'<<endl;
                return 0;
            }
        }
    }
    cout<<"2"<<endl;
}
```

## E. Sasha and a Patient Friend

    unsolved

## F. Sasha and Interesting Fact from Graph Theory

    unsolved
