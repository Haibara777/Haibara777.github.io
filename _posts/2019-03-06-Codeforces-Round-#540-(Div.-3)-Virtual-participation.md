---
layout: post
title:  "Codeforces Round #540 (Div. 3) Virtual participation"
date:   2019-03-06
desc: "Codeforces Round #540 (Div. 3) Virtual participation"
keywords: "Codeforces"
categories: [Acm]
tags: [Codeforces]
icon: icon-html
---

# [比赛链接](http://codeforces.com/contest/1118)

>趁最近没啥比赛，所以打了之前的一场div3，结果自闭。。。c题爆炸，后面也没时间写，所以只做出了三道题，不过之后还是把剩下几道题自己做出了几道，除了f。。。因为昨天的div2也没打，所以去补那一场了，f暂时没时间补，以后再说吧
>
>不过div3真是难啊，除了第一题，难度都不低，但是也不高，div2就是难度升高贼快。。。。

## [A. Water Buying](http://codeforces.com/contest/1118/problem/A)

其实挺水的。。。就比较打一升水的成本就行了

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
const int N=2e5+10,M=1e5+3;
const double Pi=acos(-1.0);
int main()
{
    ios::sync_with_stdio(false);
    int q;
    cin>>q;
    while(q--)
    {
        int a,b;
        ll n;
        cin>>n>>a>>b;
        double a1=(double)a;
        double b1=(double)b/2;
        if(a1<=b1){
            cout<<(ll)a*n<<endl;;
        }
        else {
            if(n%2==0){
                cout<<(ll)(n/2)*(ll)b<<endl;
            }
            else {
                cout<<(ll)(n/2)*(ll)b+(ll)a<<endl;
            }
        }
    }
}
```

## [B. Tanya and Candies](http://codeforces.com/contest/1118/problem/B)

如果一个糖被去掉的话，对于这个糖后面所有的糖来说，之前是奇数的糖会变成偶数，之前是偶数的糖会变成奇数，所以，先求出原本的奇数和与偶数和，再从后往前找，不断更新后面的奇数和与偶数和，通过这四个值就可以计算出，去掉当前这个糖后的奇数和与偶数和，再进行比较就行了

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
const int N=2e5+10,M=1e5+3;
const double Pi=acos(-1.0);
int a[N];
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    ll tot1=0,tot2=0;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
        if(i%2==1)tot1+=a[i];
        else tot2+=a[i];
    }
    ll tot3=0,tot4=0;
    int ans=0;
    for(int i=n;i>=1;i--)
    {
        if(i%2==1){
            tot3+=a[i];
            if(tot1-tot3+tot4==tot2-tot4+tot3-a[i])ans++;
        }
        else {
            tot4+=a[i];
            if(tot2-tot4+tot3==tot1-tot3+tot4-a[i])ans++;
        }
    }
    cout<<ans<<endl;
}
```

## [C. Palindromic Matrix](http://codeforces.com/contest/1118/problem/C)

。。。坑爹题啊，wa了九次

其实不难，计算数字出现过的次数就行了，n是偶数时好整，就是n要是奇数。。。。哎

直接上代码吧，这个题难度就在代码上。。。

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
const int N=2e5+10,M=1e5+3;
const double Pi=acos(-1.0);
int s[25*25];
int a[1005];
int mat[25][25];
int main()
{
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=1;i<=n*n;i++)
    {
        cin>>s[i];
        a[s[i]]++;
    }
    int h2=0,h4=0;
    int emmm;
    for(int i=1;i<=1000;i++)
    {
        h4+=a[i]/4;
        h2+=a[i]/2;
        if(a[i]%2==1)emmm=i;
    }
    int i=1,j=1;
    if(n%2==0&&h4==(n*n)/4){
        cout<<"YES"<<endl;
        for(int k=1;k<=n*n;k++)
        {
            if(a[s[k]]){
                mat[i][j]=s[k];
                mat[n-i+1][j]=s[k];
                mat[i][n-j+1]=s[k];
                mat[n-i+1][n-j+1]=s[k];
                a[s[k]]-=4;
                j++;
                if(j==(n/2)+1){
                    i++;j=1;
                }
            }
        }
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
            {
                cout<<mat[i][j]<<((j==n)?'\n':' ');
            }
    }
    else if(n%2==1&&h4>=(n-1)*(n-1)/4&&h2>=(n-1)*(n-1)/2+n-1){
        cout<<"YES"<<endl;
        mat[n/2+1][n/2+1]=emmm;
        a[emmm]--;
        int f1=1,f2=0;
        for(int k=1;k<=n*n;k++)
        {
            if(a[s[k]]>=4){
                mat[i][j]=s[k];
                mat[n-i+1][j]=s[k];
                mat[i][n-j+1]=s[k];
                mat[n-i+1][n-j+1]=s[k];
                a[s[k]]-=4;
                j++;
                if(j==(n/2)+1){
                    i++;j=1;
                }
                if(i==(n/2)+1)break;
            }
        }
        for(int k=1;k<=n*n;k++){
            if(a[s[k]]>=2){
                if(f2){
                    mat[f1][n/2+1]=s[k];
                    mat[n-f1+1][n/2+1]=s[k];
                }
                else {
                    mat[n/2+1][f1]=s[k];
                    mat[n/2+1][n-f1+1]=s[k];
                }
                a[s[k]]-=2;
                f1++;
                if(f1==n/2+1&&f2==0){
                    f1=1;f2=1;
                }
            }
        }
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
            {
                cout<<mat[i][j]<<((j==n)?'\n':' ');
            }
    }
    else cout<<"NO"<<endl;
}
```

## [D. Coffee and Coursework (Hard Version)](http://codeforces.com/contest/1118/problem/D2)

又是两个版本的题，还是只看难的这个

这个题是后来做出来的，想了半天才想到二分，正好tags里也有二分，所以多想了一会，竟然做出来了，呜呜呜，不容易啊，终于能独立做出二分来了

这个题直接算是很难的，但是如果规定了天数，计算能不能写完却很简单，直接计算最大的页数就行了，而且天数越多，写的最大页数一定越多，所以可以直接二分答案

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
const int N=2e5+10,M=1e5+3;
const double Pi=acos(-1.0);
ll a[N],h[N];
ll m;
int n;
bool check(int x){
    int h=n/x;
    int q=n-h*x;
    int k=1;
    ll sum=0;
    for(int i=1;i<=q;i++)
    {
        sum+=max(a[k]-h,(ll)0);
        k++;
    }
    h--;
    for(int i=1;i<=n-q;i++)
    {
        sum+=max(a[k]-h,(ll)0);
        if(i%x==0)h--;
        k++;
    }
    if(sum>=m)return true;
    else return false;
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>n>>m;
    ll tot=0;
    for(int i=1;i<=n;i++){
        cin>>a[i];
        tot+=a[i];
    }
    sort(a+1,a+n+1);
    if(tot<m){
        cout<<-1<<endl;
        return 0;
    }
    int l=1,r=n,mid;
    while(l<r){
        mid=((l+r)>>1);
        if(check(mid)){
            r=mid;
        }
        else l=mid+1;
    }
    cout<<l<<endl;
}
```

## [E. Yet Another Ball Problem](http://codeforces.com/contest/1118/problem/E)

水得一匹得题。。。随便一构造就过了我去，就是中间爆了次long long比较尴尬。。

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
const int N=2e5+10,M=1e5+3;
const double Pi=acos(-1.0);
int main()
{
    ios::sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    ll m=(ll)k*(k-1);
    if(m<(ll)n){
        cout<<"NO"<<endl;
        return 0;
    }
    cout<<"YES"<<endl;
    int h=1;
    int hh=1;
    for(int i=1;i<=n;i++)
    {
        cout<<h<<' '<<((h+hh>k)?(h+hh)%k:h+hh)<<endl;
        h++;
        if(h>k){h=1;hh++;}
    }
}
```

## [F. Tree Cutting (Hard Version)](http://codeforces.com/contest/1118/problem/F2)

又是两个版本的题，我~~就不做了~~有时间再做。。