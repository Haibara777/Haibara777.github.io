>t是时候把我常用的一些板子放上去了，边用边放吧，用到什么放什么

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
#include<bitset>
#define ll  long long
#define ull unsigned long long
using namespace std;
const int N=3e4+5,M=10007;
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
int main()
{
    ios::sync_with_stdio(false);
}
```
# 基本算法

## 倍增

```c++
int p=1;
int sum=0;
while(p){
    if(a[sum+p]<=k){
        sum+=p;
        p*=2;
    }
    else p/=2;
}
cout<<sum<<endl;
```

### ST算法

```c++
int n;
int f[N][40];
int a[N];
void ST_prework(){
	for(int i=1;i<=n;i++)
		f[i][0]=a[i];
	int t=log(n)/log(2)+1;
	for(int j=1;j<t;j++){
		int l=(1<<(j-1));
		for(int i=1;i<=n-2*l+1;i++)
			f[i][j]=max(f[i][j-1],f[i+l][j-1]);
	}
}
int ST_query(int l,int r){
	int k=log(r-l+1)/log(2);
	return max(f[l][k],f[r-(1<<k)+1][k]);
}
```

# 基本数据结构

## 栈

### 单调栈

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

## Trie(字典树)

```c++
int trie[N][26],tot=1;
bool end[N];
void insert(char *str)
{
	int len=strlen(str),p=1;
	for(int k=0;k<len;k++){
		int ch=str[k]-'a';
		if(trie[p][ch]==0)trie[p][ch]=++tot;
		p=trie[p][ch];
	}
	end[p]=true;
}
bool search(char *str)
{
	int len=strlen(str),p=1;
	for(int k=0;k<len;k++){
		int ch=str[k]-'a';
		if(trie[p][ch]==0)return false;
		p=trie[p][ch];
	}
	return end[p];
}
```

### 前缀统计

```c++
//CH1601
int trie[N][26],tot=1;
int End[N];
void insert(char *str)
{
	int len=strlen(str),p=1;
	for(int k=0;k<len;k++){
		int ch=str[k]-'a';
		if(trie[p][ch]==0)trie[p][ch]=++tot;
		p=trie[p][ch];
	}
	End[p]++;
}
int search(char *str)
{
	int ans=0;
	int len=strlen(str),p=1;
	for(int k=0;k<len;k++){
		int ch=str[k]-'a';
		p=trie[p][ch];
		if(p==0)return ans;
		ans+=End[p];
	}
	return ans;
}
char c[N];
int main()
{
	int n,m;
	n=read(),m=read();
	while(n--){
		scanf("%s",c);
		insert(c);
	}
	while(m--){
		scanf("%s",c);
		printf("%d\n",search(c));
	}
}
```

### XOR

#### CH1602

```c++
int trie[N][2],tot=1;
void insert(int x){
	int p=1;
	for(int i=30;i>=0;i--){
		int h=(x>>i);
		if(trie[p][h&1]==0)trie[p][h&1]=++tot;
		p=trie[p][h&1];
	}
}
int search(int x){
	int p=1;
	int ans=0;
	for(int i=30;i>=0;i--){
		int h=(x>>i);
		if(trie[p][(h&1)^1]){
			ans+=(1<<i);
			p=trie[p][(h&1)^1];
		}
		else p=trie[p][h&1];
	}
	return ans;
}
int a[100005];
int main()
{
	int n;
	n=read();
	for(int i=1;i<=n;i++){
		a[i]=read();
	}
	insert(a[1]);
	int ans=0;
	for(int i=2;i<=n;i++){
		ans=max(ans,search(a[i]));
		insert(a[i]);
	}
	printf("%d\n",ans);
}
```

#### POJ-3764

```c++
int trie[1000005][2],tot=1;
void insert(int x){
	int p=1;
	for(int i=30;i>=0;i--){
		int h=(x>>i);
		if(trie[p][h&1]==0)trie[p][h&1]=++tot;
		p=trie[p][h&1];
	}
}
int search(int x){
	int p=1;
	int ans=0;
	for(int i=30;i>=0;i--){
		int h=(x>>i);
		if(trie[p][(h&1)^1]){
			ans+=(1<<i);
			p=trie[p][(h&1)^1];
		}
		else p=trie[p][h&1];
	}
	return ans;
}
int head[2*N],Next[2*N],ver[2*N],edge[2*N];
void add(int x,int y,int v){
	ver[++tot]=y;
	edge[tot]=v;
	Next[tot]=head[x];
	head[x]=tot;
}
int d[N];
void dfs(int x,int fa){
	for(int i=head[x];i;i=Next[i]){
		int y=ver[i],v=edge[i];
		if(y==fa)continue;
		d[y]=d[x]^v;
		dfs(y,x);
	}
}
int main()
{
	int n;
	while(~scanf("%d",&n)){
		tot=0;
		memset(head,0,sizeof(head));
		for(int i=1;i<n;i++){
			int u,v,w;
			scanf("%d%d%d",&u,&v,&w);
			add(u,v,w);
			add(v,u,w);
		}
		memset(d,0,sizeof(d));
		memset(trie,0,sizeof(trie));
		dfs(0,-1);
		int ans=0;
		tot=1;
		insert(d[0]);
		for(int i=1;i<n;i++){
			ans=max(ans,search(d[i]));
			insert(d[i]);
		}
		printf("%d\n",ans);
	}
}
```

## 二叉堆

```c++
///大根堆
int heap[N],tot=0;
void up(int p){
	while(p>1){
		if(heap[p]>heap[p>>1]){
			swap(heap[p],heap[p>>1]);
			p>>=1;
		}
		else break;
	}
}
void Insert(int val){
	heap[++tot]=val;
	up(tot);
}
int GetTop(){
	return heap[1];
}
void down(int p){
	int s=(p<<1);
	while(s<=tot){
		if(s+1<=tot&&heap[s]<heap[s+1])s++;
		if(heap[s]>heap[p]){
			swap(heap[s],heap[p]);
			p=s;s=(p>>1);
		}
		else break;
	}
}
void Extract(){
	heap[1]=heap[tot--];
	down(1);
}
void Remove(int k){
	heap[k]=heap[tot--];
	up(k),down(k);
}
```

# 搜素

## 拓扑排序

```c++
int head[N],Next[2*N],ver[2*N],tot=0;
int in[N];
void add(int x,int y){
	ver[++tot]=y;
	Next[tot]=head[x];
	head[x]=tot;
	in[y]++;
}
int a[N];
int cnt=0;
int n,m;
void topsort(){
	queue<int> Q;
	for(int i=1;i<=n;i++){
		if(in[i]==0)Q.push(i);
	}
	while(!Q.empty()){
		int x=Q.front();
		Q.pop();
		a[++cnt]=x;
		for(int i=head[x];i;i=Next[i]){
			int y=ver[i];
			if(--in[y]==0){
				Q.push(y);
			}
		}
	}
}
```

## 剪枝

### POJ-1011

```c++
int a[105],v[105],n,len,cnt;
bool dfs(int stick,int cab,int last){
	if(stick>cnt)return true;
	if(cab==len)return dfs(stick+1,0,1);
	int fail=0;
	for(int i=last;i<=n;i++){
		if(v[i])continue;
		if(cab+a[i]>len)continue;
		if(fail==a[i])continue;
		v[i]=1;
		if(dfs(stick,cab+a[i],i+1))return true;
		fail=a[i];
		v[i]=0;
		if(cab==0||cab+a[i]==len)return false;
	}
	return false;
}
int main()
{
	ios::sync_with_stdio(false);
	while(cin>>n){
		if(n==0)break;
		int sum=0,val=0;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			sum+=a[i];
			val=max(val,a[i]);
		}
		sort(a+1,a+n+1);
		reverse(a+1,a+n+1);
		for(len=val;len<=sum;len++){
			if(sum%len)continue;
			cnt=sum/len;
			memset(v,0,sizeof(v));
			if(dfs(1,0,1))break;
		}
		cout<<len<<endl;
	}
}
```

## 迭代加深

```c++
int n;
int a[105],v[1050];
int m;
bool dfs(int k,int vv){
	if(vv==n)return true;
	if(vv>n)return false;
	if(k>m)return false;
	for(int i=k;i>=1;i--){
		for(int j=i;j>=1;j--){
			if(v[a[i]+a[j]]||a[i]+a[j]>n)continue;
			if(a[i]+a[j]<=a[k])break;
			a[k+1]=a[i]+a[j];
			v[a[i]+a[j]]=1;
			if(dfs(k+1,a[i]+a[j]))return true;
			a[k+1]=0;
			v[a[i]+a[j]]=0;
		}
	}
	return false;
}
int main()
{
	//ios::sync_with_stdio(false);
	while(~scanf("%d",&n)&&n){
		if(n==1){
			cout<<1<<endl;
			continue;
		}
		if(n==2){
			cout<<"1 2"<<endl;
			continue;
		}
		for(m=2;m<=10;m++){
			memset(a,0,sizeof(a));
			memset(v,0,sizeof(v));
			v[1]=1;
			a[1]=1;a[2]=2;
			if(dfs(2,1))break;
		}
		for(int i=1;i<=m;i++){
			cout<<a[i]<<' ';
		}
		cout<<n<<endl;
	}
}
```

## 双向搜索（折半搜索）

### CH-2401
```c++
int n,W,t;
int a[50];
int a1[30],a2[30];
ll num[N];
int tot=0;
void dfs1(int x,ll w){
	if(w>W)return;
	if(x>t){
		num[++tot]=w;
		return;
	}
	dfs1(x+1,w+(ll)a1[x]);
	dfs1(x+1,w);
}
ll ans=0;
void dfs2(int x,ll w){
	if(w>W)return;
	if(x>n-t){
		int p=lower_bound(num+1,num+tot+1,W-w)-num;
		if(num[p]==W-w)ans=max(ans,num[p]+w);
		else ans=max(ans,num[p-1]+w);
		return;
	}
	dfs2(x+1,w+(ll)a2[x]);
	dfs2(x+1,w);
}
int main()
{
	ios::sync_with_stdio(false);	
	cin>>W>>n;
	for(int i=1;i<=n;i++){
		cin>>a[i];
	}
	sort(a+1,a+n+1);
	reverse(a+1,a+n+1);
	t=n/2+2;
	for(int i=1;i<=t;i++)a1[i]=a[i];
	for(int i=1;i<=n-t;i++)a2[i]=a[t+i];
	dfs1(1,(ll)0);
	sort(num+1,num+tot+1);
	tot=unique(num+1,num+tot+1)-num-1;
	dfs2(1,(ll)0);
	cout<<ans<<endl;
}
```

# 数据结构进阶

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
int n,m;
struct node{
    int l,r,ans;
}query[N];
int a[N],fa[N],d[N];
void read_discrete(){
    cin>>n>>m;
    int t=0;
    for(int i=1;i<=m;i++){
        cin>>query[i].l>>query[i].r;
        string s;
        cin>>s;
        query[i].ans=((s[0]=='o')?1:0);
        a[++t]=query[i].l;
        a[++t]=query[i].r;
    }
    sort(a+1,a+t+1);
    n=unique(a+1,a+t+1)-a-1;
}
int get(int x){
    if(x==fa[x])return x;
    int root=get(fa[x]);
    d[x]^=d[fa[x]];
    return fa[x]=root;
}
int main()
{
    ios::sync_with_stdio(false);
    read_discrete();
    for(int i=1;i<=n;i++)fa[i]=i;
    for(int i=1;i<=m;i++)
    {
        int x=lower_bound(a+1,a+n+1,query[i].l-1)-a;
        int y=lower_bound(a+1,a+n+1,query[i].r)-a;
        int p=get(x),q=get(y);
        if(p==q){
            if((d[x]^d[y])==query[i].ans)continue;
            cout<<i-1<<endl;
            return 0;
        }
        fa[p]=q;
        d[p]=d[x]^d[y]^query[i].ans;
    }
    cout<<m<<endl;
}

```

### 扩展域

```c++
int n,m;
struct node{
    int l,r,ans;
}query[N];
int fa[N*2];
int a[N];
void read_discrete(){
    cin>>n>>m;
    int t=0;
    for(int i=1;i<=m;i++){
        cin>>query[i].l>>query[i].r;
        string s;
        cin>>s;
        query[i].ans=((s[0]=='o')?1:0);
        a[++t]=query[i].l;
        a[++t]=query[i].r;
    }
    sort(a+1,a+t+1);
    n=unique(a+1,a+t+1)-a-1;
}
int get(int x){
    if(x==fa[x])return x;
    return fa[x]=get(fa[x]);
}
int main()
{
    ios::sync_with_stdio(false);
    read_discrete();
    for(int i=1;i<=2*n;i++)fa[i]=i;
    for(int i=1;i<=m;i++)
    {
        int x=lower_bound(a+1,a+n+1,query[i].l-1)-a;
        int y=lower_bound(a+1,a+n+1,query[i].r)-a;
        int x_odd=x,x_even=x+n;
        int y_odd=y,y_even=y+n;
        if(query[i].ans==0){
            if(get(x_odd)==get(y_even)){
                cout<<i-1<<endl;
                return 0;
            }
            fa[get(x_odd)]=get(y_odd);
            fa[get(x_even)]=get(y_even);
        }
        else{
            if(get(x_even)==get(y_even)){
                cout<<i-1<<endl;
                return 0;
            }
            fa[get(x_odd)]=get(y_even);
            fa[get(x_even)]=get(y_odd);
        }
    }
    cout<<m<<endl;
}
```

## 线段树

### 区间开根号

```c++
//hdu-4027
ll a[N];
struct SegmentTree{
	int l,r;
	ll sum;
	#define l(x) tree[x].l
	#define r(x) tree[x].r
	#define sum(x) tree[x].sum
}tree[N*4];
void build(int p,int l,int r){
	l(p)=l,r(p)=r;
	if(l==r){
		sum(p)=a[l];
		return;
	}
	int mid=((l+r)>>1);
	build(p<<1,l,mid);
	build(p<<1|1,mid+1,r);
	sum(p)=sum(p<<1)+sum(p<<1|1);
}
void change(int p,int l,int r){
	if(r(p)-l(p)+1==sum(p)){
		return;
	}
	if(l<=l(p)&&r>=r(p)&&l(p)==r(p)){
		sum(p)=sqrt(sum(p));
		return;
	}
	int mid=((l(p)+r(p))>>1);
	if(l<=mid)change(p<<1,l,r);
	if(r>mid)change(p<<1|1,l,r);
	sum(p)=sum(p<<1)+sum(p<<1|1);
}
ll ask(int p,int l,int r){
	if(l<=l(p)&&r>=r(p)){
		return sum(p);
	}
	ll ans=0;
	int mid=((l(p)+r(p))>>1);
	if(l<=mid)ans+=ask(p<<1,l,r);
	if(r>mid)ans+=ask(p<<1|1,l,r);
	return ans;
}
int main(){
	int n;
	int t=0;
	while(~scanf("%d",&n)){
		t++;
		for(int i=1;i<=n;i++){
			a[i]=read();
		}
		build(1,1,n);
		int m;
		printf("Case #%d:\n",t);
		scanf("%d",&m);
		while(m--){
			int e,l,r;
			scanf("%d%d%d",&e,&l,&r);
			if(r<l){
				swap(l,r);
			}
			if(e==1){
				printf("%I64d\n",ask(1,l,r));
			}
			else {
				change(1,l,r);
			}
		}
		puts("");
	}
}
```

## 树状数组

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
```c++
void discrete(){
    sort(a+1,a+t+1);
    n=unique(a+1,a+t+1)-(a+1);
}
int id(int x){
    return lower_bound(a+1,a+n+1,x)-a;
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

# 图论

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

## lca（倍增）

```c++
int f[N][20],d[N],dist[N],t;
int ver[2*N],Next[N*2],edge[N*2],head[N],tot=0;
void add(int x,int y,int v){
	ver[++tot]=y;
	edge[tot]=v;
	Next[tot]=head[x];
	head[x]=tot;
}
void bfs(){
	queue<int> Q;d[1]=1;Q.push(1);dist[1]=0;
	while(!Q.empty()){
		int x=Q.front();Q.pop();
		for(int i=head[x];i;i=Next[i]){
			int y=ver[i],w=edge[i];
			if(d[y])continue;
			d[y]=d[x]+1;
			dist[y]=dist[x]+w;
			f[y][0]=x;
			for(int j=1;j<=t;j++)
				f[y][j]=f[f[y][j-1]][j-1];
			Q.push(y);
		}
	}
}
int lca(int x,int y){
	if(d[x]>d[y])swap(x,y);
	for(int i=t;i>=0;i--)
		if(d[f[y][i]]>=d[x])y=f[y][i];
	if(x==y)return x;
	for(int i=t;i>=0;i--)
		if(f[y][i]!=f[x][i])y=f[y][i],x=f[x][i];
	return f[x][0];
}
int main()
{
	ios::sync_with_stdio(false);
	int T;
	cin>>T;
	while(T--){
		int n,m;tot=0;
		cin>>n>>m;
		t=(int)(log(n)/log(2))+1;
		memset(head,0,sizeof(head));
		memset(d,0,sizeof(d));
		for(int i=1;i<n;i++){
			int x,y,v;
			cin>>x>>y>>v;
			add(x,y,v),add(y,x,v);
		}
		bfs();
		while(m--){
			int x,y;
			cin>>x>>y;
			int root=lca(x,y);
			cout<<dist[x]+dist[y]-2*dist[root]<<endl;
		}
	}
}
```

# 杂项

## 线性筛

```c++
ll phi[N+10];
bool vis[N+10];
ll prime[N+10];
ll miu[N+10];
ll tot;
void init(){
    miu[1]=1;
    tot = 0;
    for(ll i = 2; i <=N; i ++)
    {
        if(!vis[i])
        {
            prime[tot ++] = i;
            phi[i] = i - 1;
            miu[i]=-1;
        }
        for(ll j = 0; j < tot; j ++)
        {
            if(i * prime[j] >= N) break;
            vis[i * prime[j]] = 1;
            if(i % prime[j] == 0)
            {
                miu[i*prime[j]]=0;
                phi[i * prime[j]] = phi[i] * prime[j];
                break;
            }
            else
            {
                phi[i * prime[j]] = phi[i] * phi[prime[j]];
                miu[i*prime[j]]=-1*miu[i];
            }
        }
    }
}
```

## 矩阵快速幂

```c++
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

```c++
struct mat
{
    int a[5][5];
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
mat mul(mat y,mat x)
{
    mat ans;
    ans.set(0);
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
            for(int k=1;k<=n;k++)
            {
                ans.a[i][j]+=x.a[i][k]*y.a[k][j]%M;
                ans.a[i][j]%=M;
            }
    return ans;
}
mat mqpow(mat x,int y){
    mat h=x,ans;
    ans.set(0);
    ans.a[4][1]=2;
    ans.a[3][1]=4;
    ans.a[2][1]=6;
    ans.a[1][1]=9;
    while(y){
        if(y&1)ans=mul(ans,h);
        h=mul(h,h);
        y>>=1;
    }
    return ans;
}
int main()
{
    //ios::sync_with_stdio(false);
    int L;
    mat p;
    n=4;
    p.set(0);
    p.a[1][1]=p.a[1][3]=p.a[1][4]=1;
    p.a[2][1]=p.a[3][2]=p.a[4][3]=1;
    while(~scanf("%d%d",&L,&M)){
        if(L<=4){
            if(L==0)printf("0\n");
            if(L==1)printf("%d\n",2%M);
            if(L==2)printf("%d\n",4%M);
            if(L==3)printf("%d\n",6%M);
            if(L==4)printf("%d\n",9%M);
        }
        else{
            mat ans=mqpow(p,L-4);
            printf("%d\n",ans.a[1][1]%M);
        }
    }
}
```

## 扩栈

```c++
int size = 256 << 20; // 256MB
char *p = (char*)malloc(size) + size;
__asm__("movl %0, %%esp\n" :: "r"(p));
```

## 高精度

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