# Arknights_OCR_Material-design-2-APP
明日方舟MD2-OCR脚本软件  
#这是一个设想中的项目  
因为作者没有时间233333



# SCAU青训队第一次训练题解

## Problem A：筛素数

通过读题，只有$w$是质数，$1-w$与它互质的正整数为$w-1$个。所以题目要求既是打印$1$到$w$中的所有质数。

```c
#include<iostream>
using namespace std;
const int N=2e6+6;
int n,prime,primes[N];
bool f[N];
int main()
{
	cin>>n;
	for(int i=2;i<=n;i++){
		if(!f[i])  primes[++prime]=i;
		for(int j=1;primes[j]*i<=n;j++){
			f[i*primes[j]]=true;
			if(i%primes[j]==0) break;
		}
	}
	for(int i=1;i<=prime;i++)
	  cout<<primes[i]<<'\n';
	return 0;
}
```



## Problem B：简单数学、思维

每一项取出一个$k^2$出来后，等式变为求$k^2\times(1^2+2^2+\dots+\left \lfloor \frac{n}{k}\right \rfloor ^2 )$的值。

```c
#include<iostream>
using namespace std;
int T;
int main()
{
	scanf("%d",&T);
	long long n,k;
	while(T--)
	{
		scanf("%lld%lld",&n,&k);
		n=n/k;
		long long ans=k*k*n*(n+1)*(2*n+1)/6;
		printf("%lld\n",ans);
	}
	return 0;
}
```



## Problem C：模拟题

简单模拟题，注意输出格式。

按照分数排序，随后求出至少能作出多大的矩阵，每行输出对应长度的字符串即可。不够一行的就在最后一行排就行。

```c
#include<iostream>
#include<algorithm>
#include<math.h>
using namespace std;
int n;
struct C{
	int score;
	char name[25];
}s[2005];
bool cmp(C i,C j){
	if(i.score == j.score )
	  return i.name > j.name;
	return i.score > j.score;
}
int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++){
		scanf("%s %d",s[i].name,&s[i].score);
	}
	sort(s+1,s+n+1,cmp);
	int k=sqrt(n);
	printf("%d\n",k);
	for(int i=1;i<=n;i++){
		printf("%-6.6s",s[i].name);
		if(i%k==0) printf("\n");
	}
	return 0;
}
```



## Problem D：模拟题

注意输入，包含空格的字符串。

随后用'  '替换成'f'，很明显，每一段字符串后面有2个换行，所以在处理完每一段字符串后输出'q'即可。

```c
#include<string>
#include<iostream>
using namespace std;
int main()
{
	string a,b;
	getline(cin,a);
	getline(cin,b);
	for(int i=0;i<a.size();i++)
	  if(a[i]==' ') a[i]='f';
	for(int i=0;i<b.size();i++)
	  if(b[i]==' ') b[i]='f';
	cout<<a<<'q'<<b<<'q';
	return 0;
}
```



## Problem E：数据结构——树状数组

做两遍树状数组，第一遍记录位置为$i$的这个点左边比它小的有多少个数，第二遍记录右边比它大的有多少个数，那么以$i$这个点为中间值，符合条件的个数即为两边的个数相乘。

每一位本身的值对于结果是没有影响的，有影响的是各个数之间的大小关系。所以可以先离散化之后再做树状数组。

```c
#include<iostream>
#include<stdio.h>
#include<algorithm>
using namespace std;
struct TREE{
	int n;
	int w;
}a[1000005];
int N,b[1000005],t[2][1000005];
long long Ans;
int l[1000005],r[1000005];
bool cmp(TREE i,TREE j)
{
	return i.n<j.n;
}
int low_bite(int x)
{
	return x & -x ;
}
void in_(int f,int x)
{
	while(x<=N)
	{
		t[f][x]++;
		x+=low_bite(x);
	}
	return ;
}
int get_ans(int f,int x)
{
	int ans=0;
	while(x)
	{
		ans+=t[f][x];
		x-=low_bite(x);
	}
	return ans;
}
void read();
int main()
{
	read();
	for(int i=N;i>=1;i--)
	{
		in_(0,b[i]);
		l[i]=get_ans(0,b[i]-1);
	}
	for(int i=1;i<=N;i++)
	{
		in_(1,b[i]);
		r[i]=i-get_ans(1,b[i]);
	}
	for(int i=2;i<N;i++)
	  Ans+=(long long)l[i]*r[i];
	printf("%I64d",Ans);
	return 0;
}
void read()
{
	scanf("%d",&N);
	for(int i=1;i<=N;i++)
	{
		scanf("%d",&a[i].n);
		a[i].w=i;
	}
	sort(a+1,a+N+1,cmp);
	for(int i=1;i<=N;i++)
	  b[a[i].w]=i;
	return ;
}
```



## Problem F：数据结构——堆

通过分析，每一次当前拥有的堆的重量中最轻的两个合并，能够使得最后所消耗的体力之和最小。

那么可以用优先队列，每次取出最小的两个值，相加之后再放入优先队列中，然后循环取二、相加、入队的操作，知道队伍中只剩下一个元素。

```c
#include<iostream>
#include<queue>
#include<stdio.h>
using namespace std;
int n;
priority_queue<int,vector<int>,greater<int> > q;
int main()
{
	cin>>n;
	for(int i=1;i<=n;i++){
		int x;
		cin>>x;
		q.push(x);
	}
	int ans=0;
	while(q.size()>1){
		int a=q.top();q.pop();
		int b=q.top();q.pop();
		q.push(a+b); 
		ans+=a+b;
	}
	cout<<ans;
	return 0;
}
```



## Problem G：简单数学、思维

假设只有三个数$a,b,c$，操作一次后为$ab+1,c$，再操作一遍为$abc+c$，由此可见，要让最后的值最小，那么这个$c$要是最小值。那么推广到$n$个数也是如此。所以排序后每次与最大的值相乘+1即可。

```c
#include<iostream>
#include<algorithm>
#include<stdio.h>
using namespace std;
const int mod=1000000007;
int n,ans;
long long a[205];
int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++)
	  scanf("%d",&a[i]);
	sort(a+1,a+n+1);
	ans=a[n];
	for(int i=n-1;i>=1;i--)
	{
		ans=ans%mod*a[i]%mod+1;
		ans=ans%mod;
	}
	printf("%d",ans);
	return 0;
}
```



## Problem H：二分答案

经典的二分答案的题目。

首先，答案的范围为$[1,\sum{a_i}]$，每次check一下中间值mid是否是区间中最大的最小值，如果符合条件，mid可能就是答案，也可能还有比mid小的值满足条件，所以再进行多一次二分，直到找到最精确的答案。

```c
#include<iostream>
#include<stdio.h>
using namespace std;
int n,m,l,r,mid;
int a[100005];
bool check_(int k){
	int res=0,cnt=0;
	for(int i=1;i<=n;i++){
		if(res+a[i]>k){
			cnt++;
			res=a[i];
		}
		else{
			res+=a[i];
		}
	}
	if(res<=k) cnt++;
	if(cnt<=m) return true;
	else return false;
}
int main()
{
	freopen("H.in","r",stdin);
	freopen("H.out","w",stdout);
	cin>>n>>m;
	for(int i=1;i<=n;i++) cin>>a[i],r+=a[i];
	while(l<r){
		mid=l + r >> 1;
		if(check_(mid)) r=mid;
		else l=mid+1;
	}
	cout<<l;
	return 0;
}
```



## Problem I：差分、贪心

我们发现，若$a_{i+1}>a_i$，那么我们必须多铺上$a_{i+1}-a_i$的高度。

那么依照差分的思想，构造差分数组$d_i=a_i-a_{i-1}$，若差分值为正数，则相加即为结果。

```c
#include<iostream>
#include<stdio.h>
using namespace std;
int n,a[1000005],d[1000005],ans;
int main()
{
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>a[i];
		d[i]=a[i]-a[i-1];
		if(d[i]>0) ans+=d[i];
	}
	cout<<ans;
	return 0;
}
```

还有一种方法，既是贪心。

一开始是1到n这个区间，我们需要去掉这个区间的最小值，并且其他都同时减去这个最小值。随后就变成了两个区间。其他区间也进行找最小值的操作。直至只有一个数为止。

```c
#include<iostream>
#include<stdio.h>
#include<string>
#include<cstring>
#include<cstdio>
#include<math.h>
#include<cmath>
#include<stdlib.h>
#include<algorithm>
#include<queue>
using namespace std;
const int N=100005;
struct Road{
	int l,r;
};
queue<Road> q;
int n,a[N],ans;
void read();
void read_into(int L,int R);
void work();
int find_min(int L,int R);
int main()
{
	read();
	work();
	return 0;
}
void work()
{
	while(!q.empty())
	{
		Road now=q.front();q.pop();
		int h=now.l,t=now.r,mid=find_min(h,t),rue=a[mid];
		ans+=rue;
		for(int i=h;i<=t;i++)
		  a[i]-=rue;
		if(h==t) continue;
		if(h<=mid-1) read_into(h,mid-1);
		if(t>=mid+1) read_into(mid+1,t);
	}
	printf("%d",ans);
}
int find_min(int L,int R)
{
	int num=0;
	for(int i=L;i<=R;i++)
	  if(a[i]<a[num])
	    num=i;
	return num;
}
void read()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++)
	  scanf("%d",&a[i]);
	a[0]=100005;
	read_into(1,n);
}
void read_into(int L,int R)
{
	Road add_in;
	add_in.l=L;add_in.r=R;
	q.push(add_in);
}
```



## Problem J：完全背包

可以观察到，最大的$a_i$才25000，那么我们可以做一个容量为25000的背包。

先将所有的货币按价格从小到大排序，从最小的货币开始做完全背包，一路去算出它能够凑成的货币额度。然后再从下一个货币开始凑。若这个货币已经被之前的货币所表示了，说明这个货币是可以被取代，即不需要了。

最后看一下一共用了多少种货币即可。

```c
#include<iostream>
#include<stdio.h>
#include<string.h>
#include<algorithm>
using namespace std;
int T,n,a[105];
bool f[25005];
int main()
{
	cin>>T;
	while(T--){
		int maxn=0,ans=0;
		cin>>n;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			maxn=max(maxn,a[i]);
		}
		sort(a+1,a+n+1);
		memset(f,false,sizeof(f));
		f[0]=true;
		for(int i=1;i<=n;i++){
			if(!f[a[i]]){
				ans++;
				for(int j=0;j<=maxn;j++){
					if(f[j] && j+a[i]<=maxn){
						f[j+a[i]]=true;
					}
				}
			}
		}
		cout<<ans<<'\n';
	}
	return 0;
}
```



