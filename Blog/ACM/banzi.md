# 板子娘们

>是时候把我常用的一些板子放上去了，边用边放吧，用到什么放什么

```c++
#include<iostream>
#include<iomanip>
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
#define ll  long long
#define ull unsigned long long
using namespace std;
const int N=2e4+5,M=10007;
const ull base=13331;
const double Pi=acos(-1.0);
const ll C=299792458;
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
int sum(int x){
    int ans=0;
    for(int i=x;i>=1;i-=lowbit(i))ans=ans+h[i];
    return ans;
}
void add(int x,int y)
{
    for(int i=x;i<=N;i+=lowbit(i))
        h[i]=h[i]+y;
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
            for(int j=1;j<=m;j++)
                a[i][j]=(i==j)?1:0;
    }
    void set(ll k){
        for(int i=1;i<=n;i++)
            for(int j=1;j<=m;j++)
                a[i][j]=k;
    }
};
mat mul(mat y,mat x)    //矩阵A*B=mul(A,B)，矩阵B*A=mul(B,A)
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
    /*for(int i=1;i<=ans.n;i++)
    {
        ans.a[11-i][1]=i-1;
    }*/                       ////可以规定第一列的元素，否则ans就直接等于x
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

## 树的直径

### dp

```c++
int head[N],Next[N],ver[N],edge[N],tot=0;
void add(int x,int y,int v)
{
    ver[++tot]=y;
    edge[tot]=v;
    Next[tot]=head[x];
    head[x]=tot;
}
int d[N];
int ans=0;
int v[N];
void dp(int x)
{
    v[x]=1;
    for(int i=head[x];i;i=Next[i])
    {
        int y=ver[i];
        if(v[y])continue;
        v[y]=1;
        dp(y);
        ans=max(ans,d[x]+d[y]+edge[i]);
        d[x]=max(d[x],d[y]+edge[i]);
    }
}
int main()
{
    int u,v,w;
    while(scanf("%d%d%d",&u,&v,&w)!=EOF){
        add(u,v,w);
        add(v,u,w);
    }
    dp(1);
    cout<<ans<<endl;
}
```

### 两次bfs

```c++
int head[N],Next[N],ver[N],edge[N],tot=0;
void add(int x,int y,int v)
{
    ver[++tot]=y;
    edge[tot]=v;
    Next[tot]=head[x];
    head[x]=tot;
}
int v[N];
struct node{
    int d;
    int p;
};
int s;
int bfs(int pp)
{
    queue<node> Q;
    Q.push({0,pp});
    int ans=0;
    memset(v,0,sizeof(v));
    v[pp]=1;
    while(!Q.empty())
    {
        node c=Q.front();
        Q.pop();
        int x=c.p;
        for(int i=head[x];i;i=Next[i])
        {
            int y=ver[i];
            int w=edge[i];
            if(v[y])continue;
            v[y]=1;
            Q.push({c.d+w,y});
            if(c.d+w>ans){
                ans=c.d+w;
                s=y;
            }
        }
    }
    return ans;
}
int main()
{
    int u,v,w;
    while(scanf("%d%d%d",&u,&v,&w)!=EOF){
        add(u,v,w);
        add(v,u,w);
    }
    bfs(1);
    cout<<bfs(s)<<endl;
}
```

## 分块

```c++
ll a[N],L[N],R[N],sum[N],pos[N],add[N],t;
void change(int l,int r,ll d)
{
    int p=pos[l],q=pos[r];
    if(p==q){
        for(int i=l;i<=r;i++){
            a[i]+=d;
            sum[p]+=d;
        }
        return;
    }
    for(int i=p+1;i<=q-1;i++)add[i]+=d;
    for(int i=l;i<=R[p];i++){a[i]+=d;sum[p]+=d;}
    for(int i=L[q];i<=r;i++){a[i]+=d;sum[q]+=d;}
}
ll ask(int l,int r)
{
    ll ans=0;
    int p=pos[l],q=pos[r];
    if(p==q){
        for(int i=l;i<=r;i++)
            ans+=a[i];
        ans+=add[p]*(r-l+1);
        return ans;
    }
    for(int i=p+1;i<=q-1;i++)
        ans+=sum[i]+add[i]*(R[i]-L[i]+1);
    for(int i=l;i<=R[p];i++)
        ans+=a[i]+add[p];
    for(int i=L[q];i<=r;i++)
        ans+=a[i]+add[q];
    return ans;
}
int main()
{
    ios::sync_with_stdio(false);
    int n,q;
    scanf("%d%d",&n,&q);
    for(int i=1;i<=n;i++)
        scanf("%I64d",&a[i]);
    t=sqrt(n);
    for(int i=1;i<=t;i++)
    {
        L[i]=(i-1)*t+1;
        R[i]=i*t;
    }
    if(R[t]<n){
        t++;
        L[t]=R[t-1]+1;
        R[t]=n;
    }
    for(int i=1;i<=t;i++)
    {
        for(int j=L[i];j<=R[i];j++)
        {
            pos[j]=i;
            sum[i]+=a[j];
        }
    }
    while(q--){
        int l,r;
        char s[10];
        scanf("%s%d%d",s,&l,&r);
        if(s[0]=='Q'){
            cout<<ask(l,r)<<endl;
        }
        else {
            ll d;
            scanf("%I64d",&d);
            change(l,r,d);
        }
    }
}
```

## 并查集

### 边带权

```c++
//CH4101/NOI2002
int fa[N];
int d[N],size[N]; //d[x]为x到fa[x]的距离，size[x]为以x为根节点的树的大小，初始全为1
int get(int x){
    if(x==fa[x])return x;
    int root=get(fa[x]);
    d[x]+=d[fa[x]];
    return fa[x]=root;
}
void merge(int x,int y){
    x=get(x),y=get(y);
    fa[x]=y;
    d[x]=size[y];
    size[y]+=size[x];
}
```

```c++

```

## 点分治

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
#define ll  long long
#define ull unsigned long long
using namespace std;
const int N=2e4+5,M=10007;
const ull base=13331;
const double Pi=acos(-1.0);
const ll C=299792458;
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
int head[N],Next[N],ver[N],edge[N],tot=0;
void add(int x,int y,int v)
{
    ver[++tot]=y;
    edge[tot]=v;
    Next[tot]=head[x];
    head[x]=tot;
}
int v[N];
ll size[N];
int pos;
int n;
int d[N];
int b[N];
int sn;
void dfs_root(int x,int fa)
{
    d[x]=0;
    b[x]=1;
    for(int i=head[x];i;i=Next[i])
    {
        int y=ver[i];
        if(fa==y||v[y])continue;
        dfs_root(y,x);
        b[x]=b[x]+b[y];
        d[x]=max(d[x],b[y]);
    }
    d[x]=max(d[x],sn-b[x]);
    if(d[pos]>d[x]){
        pos=x;
    }
}
int num=0;
int dis[N];
void dfs_dis(int x,int d,int fa)
{
    dis[++num]=d;
    for(int i=head[x];i;i=Next[i])
    {
        int y=ver[i];
        if(y==fa||v[y])continue;
        dfs_dis(y,d+edge[i],x);
    }
}
int calc(int x,int d)
{
    num=0;
    dfs_dis(x,d,x);
    sort(dis+1,dis+num+1);
    int l=1,r=num;
    int cnt=0;
    while(l<r){
        while(dis[l]+dis[r]>k&&l<r)r--;
        cnt+=r-l;
        l++;
    }
    return cnt;
}
ll ans=0;
void dfs(int x)
{
    pos=0;
    dfs_root(x,0);
    v[pos]=1;
    ans+=(ll)calc(pos,0);
    for(int i=head[pos];i;i=Next[i])
    {
        int y=ver[i];
        if(v[y])continue;
        ans-=(ll)calc(y,edge[i]);
        sn=b[y];
        dfs(y);
    }
}
int main()
{
    while(~scanf("%d%d",&n,&k))
    {
        if(n==0&&k==0)break;
        tot=0;
        memset(v,0,sizeof(v));
        memset(head,0,sizeof(head));
        for(int i=1;i<=n-1;i++)
        {
            int x,y,z;
            scanf("%d%d%d",&x,&y,&z);
            add(x,y,z);
            add(y,x,z);
        }
        sn=n;
        ans=0;
        d[0]=0x7fffffff;
        dfs(1);
        printf("%I64d\n",ans);
    }
}
```

## 离散化

### 一维

```c++
struct node{
    int id;
    ll v;
    friend bool operator <(node m,node n)
    {
        return m.v<n.v;
    }
};
vector<node> t;
int main()
{
    sort(t.begin(),t.end());
    int s=1;
    a[t[0].id]=1;
    for(int i=1;i<(int)t.size();i++)
    {
        if(t[i].v!=t[i-1].v)s++;
        a[t[i].id]=s;
    }
}
```

### 二维

```c++
struct node{
    int idx,idy;
    ll v;
    friend bool operator <(node m,node n)
    {
        return m.v<n.v;
    }
};
vector<node> t;
int main()
{
    sort(t.begin(),t.end());
    int s=1;
    a[t[0].idx][t[0].idy]=1;
    for(int i=1;i<(int)t.size();i++)
    {
        if(t[i].v!=t[i-1].v)s++;
        a[t[i].idx][t[i].idy]=s;
    }
}
```

## 单调栈

```c++
int p;
memset(s,0,sizeof(s));
a[n+1]=p=0;
ll ans=0;
for(int i=1;i<=n+1;i++)
{
    if(a[i]>s[p]){
        s[++p]=a[i];
        w[p]=1;
    }
    else{
        int width=0;
        while(a[i]<s[p]){
            width+=w[p];
            ans=max(ans,(ll)width*s[p]);
            p--;
        }
        w[++p]=width+1;
        s[p]=a[i];
    }
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