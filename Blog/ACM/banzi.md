# 板子娘们

>是时候把我常用的一些板子放上去了，边用边放吧，用到什么放什么

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
const int N=4e4+5,M=1e9+7;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
```

## 读入挂娘

```c++
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
```

## 树状数组娘

### 一维
```c++
inline int lowbit(int x)
{
    return x&(-x);
}
```
### 二维
```c++

```

## 线性筛娘

```c++
```

## 矩阵快速幂娘

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
const int N=1005,M=1e9+7;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
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
```