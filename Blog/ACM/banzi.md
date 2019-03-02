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
const int N=2e4+10,M=1e5+3;
const double Pi=acos(-1.0);
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

## 高精度娘

```c++
#include<bits/stdc++.h>
using namespace std;
const int maxn=10005;/*精度位数,自行调整*/
//1.如果需要控制输出位数的话，在str()里面把len调成需要的位数
//2.很大的位数是会re的，所以如果是幂运算的话，如 计算x^p的位数n, n=p*log(10)x+1;(注意要加一）
//3.还可以加上qmul，取模的过程也就是str()，c_str()再搞一次
class bign
{
    //io*2 bign*5*2 bool*6
    friend istream& operator>>(istream&,bign&);
    friend ostream& operator<<(ostream&,const bign&);
    friend bign operator+(const bign&,const bign&);
    friend bign operator+(const bign&,int&);
    friend bign operator*(const bign&,const bign&);
    friend bign operator*(const bign&,int&);
    friend bign operator-(const bign&,const bign&);
    friend bign operator-(const bign&,int&);
    friend bign operator/(const bign&,const bign&);
    friend bign operator/(const bign&,int&);
    friend bign operator%(const bign&,const bign&);
    friend bign operator%(const bign&,int&);
    friend bool operator<(const bign&,const bign&);
    friend bool operator>(const bign&,const bign&);
    friend bool operator<=(const bign&,const bign&);
    friend bool operator>=(const bign&,const bign&);
    friend bool operator==(const bign&,const bign&);
    friend bool operator!=(const bign&,const bign&);

private://如果想访问len,改成public
    int len,s[maxn];
public:
    bign()
    {
        memset(s,0,sizeof(s));
        len=1;
    }
    bign operator=(const char* num)
    {
        int i=0,ol;
        ol=len=strlen(num);
        while(num[i++]=='0'&&len>1)
            len--;
        memset(s,0,sizeof(s));
        for(i=0; i<len; i++)
            s[i]=num[ol-i-1]-'0';
        return *this;
    }
    bign operator=(int num)
    {
        char s[maxn];
        sprintf(s,"%d",num);
        *this=s;
        return *this;
    }
    bign(int num)
    {
        *this=num;
    }
    bign(const char* num)
    {
        *this=num;
    }
    string str() const
    {
        string res="";
        for(int i=0; i<len; i++)
            res=char(s[i]+'0')+res;
        if(res=="")
            res="0";
        return res;
    }
};
bool operator<(const bign& a,const bign& b)
{
    int i;
    if(a.len!=b.len)
        return a.len<b.len;
    for(i=a.len-1; i>=0; i--)
        if(a.s[i]!=b.s[i])
            return a.s[i]<b.s[i];
    return false;
}
bool operator>(const bign& a,const bign& b)
{
    return b<a;
}
bool operator<=(const bign& a,const bign& b)
{
    return !(a>b);
}
bool operator>=(const bign& a,const bign& b)
{
    return !(a<b);
}
bool operator!=(const bign& a,const bign& b)
{
    return a<b||a>b;
}
bool operator==(const bign& a,const bign& b)
{
    return !(a<b||a>b);
}
bign operator+(const bign& a,const bign& b)
{
    int up=max(a.len,b.len);
    bign sum;
    sum.len=0;
    for(int i=0,t=0;t||i<up; i++)
    {
        if(i<a.len)
            t+=a.s[i];
        if(i<b.len)
            t+=b.s[i];
        sum.s[sum.len++]=t%10;
        t/=10;
    }
    return sum;
}
bign operator+(const bign& a,int& b)
{
    bign c=b;
    return a+c;
}
bign operator*(const bign& a,const bign& b)
{
    bign res;
    for(int i=0; i<a.len; i++)
    {
        for(int j=0; j<b.len; j++)
        {
            res.s[i+j]+=(a.s[i]*b.s[j]);
            res.s[i+j+1]+=res.s[i+j]/10;
            res.s[i+j]%=10;
        }
    }
    res.len=a.len+b.len;
    while(res.s[res.len-1]==0&&res.len>1)
        res.len--;
    if(res.s[res.len])
        res.len++;
    return res;
}
bign operator*(const bign& a,int& b)
{
    bign c=b;
    return a*c;
}
//只支持大数减小数
bign operator-(const bign& a,const bign& b)
{
    bign res;
    int len=a.len;
    for(int i=0; i<len; i++)
    {
        res.s[i]+=a.s[i]-b.s[i];
        if(res.s[i]<0)
        {
            res.s[i]+=10;
            res.s[i+1]--;
        }
    }
    while(res.s[len-1]==0&&len>1)
        len--;
    res.len=len;
    return res;
}
bign operator-(const bign& a,int& b)
{
    bign c=b;
    return a-c;
}
bign operator/(const bign& a,const bign& b)
{
    int i,len=a.len;
    bign res,f;
    for(i=len-1; i>=0; i--)
    {
        f=f*10;
        f.s[0]=a.s[i];
        while(f>=b)
        {
            f=f-b;
            res.s[i]++;
        }
    }
    while(res.s[len-1]==0&&len>1)
        len--;
    res.len=len;
    return res;
}
bign operator/(const bign& a,int& b)
{
    bign c=b;
    return a/c;
}
bign operator%(const bign& a,const bign& b)
{
    int len=a.len;
    bign f;
    for(int i=len-1; i>=0; i--)
    {
        f=f*10;
        f.s[0]=a.s[i];
        while(f>=b)
            f=f-b;
    }
    return f;
}
bign operator%(const bign& a,int& b)
{
    bign c=b;
    return a%c;
}
bign& operator+=(bign& a,const bign& b)
{
    a=a+b;
    return a;
}
bign& operator-=(bign& a,const bign& b)
{
    a=a-b;
    return a;
}
bign& operator*=(bign& a,const bign& b)
{
    a=a*b;
    return a;
}
bign& operator/=(bign& a,const bign& b)
{
    a=a/b;
    return a;
}
bign& operator++(bign& a)
{
    a=a+1;
    return a;
}
bign& operator++(bign& a,int)
{
    bign t=a;
    a=a+1;
    return t;
}
bign& operator--(bign& a)
{
    a=a-1;
    return a;
}
bign& operator--(bign& a,int)
{
    bign t=a;
    a=a-1;
    return t;
}
istream& operator>>(istream &in,bign& x)
{
    string s;
    in>>s;
    x=s.c_str();
    return in;
}
ostream& operator<<(ostream &out,const bign& x)
{
    out<<x.str();
    return out;
}
int main()
{
    bign a;
    bign b;
    cin >> a>>b;
    cout << a*b;
    return 0;
}
```