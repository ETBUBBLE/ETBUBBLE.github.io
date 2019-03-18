---
title: Codeforces Round 528 (Div. 2, based on Technocup 2019 Elimination Round 4)
date: 2018-12-24 11:30:02
tags: CSDN迁移
---
  随手写一篇博客吧0.0

 
# [A. Right-Left Cipher](http://codeforces.com/contest/1087/problem/A)

 直接模拟，偶数在左边，奇数在右边。

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>
#include<bitset>

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> P;

#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));

const long long mod=1e9+7;
const int maxn=1e6+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;

char s[maxn],t[maxn];
int main() {
    cin>>t;
    int l=strlen(t);
    int k=l-1;
    if(l&1)k++;
    for(int i=0; i<(l+1)/2; i++) {
        s[k]=t[i];
        k-=2;
    }
    k=2;
    for(int i=(l+1)/2; i<l; i++) {
        s[k]=t[i];
        k+=2;
    }
    for(int i=1; i<=l; i++)printf("%c",s[i]);
    return 0;
}
```
 
# [B. Div Times Mod](http://codeforces.com/contest/1087/problem/B)

 直接暴力枚举啊，不就是 a*k+b=x b=n/a,直接暴力枚举n的因子。

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>
#include<bitset>

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> P;

#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));

const long long mod=1e9+7;
const int maxn=1e6+5;
const int INF=0x7fffffff;
const LL inf=0x3f3f3f3f;
const double eps=1e-8;
LL n,k;
int main() {
    cin>>n>>k;
    LL ans=inf*inf;
    for(LL i=1; i<=n; i++) {
        if(n%i==0&&n/i<k) {
            ans=min(ans,k*i+n/i);
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
 
# [C. Connect Three](http://codeforces.com/contest/1087/problem/C)

 直接想象怎么走最近，随便瞎几把写，数据有点水，我数组开小了都过了，结果被fst了。

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>
#include<bitset>

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> P;

#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));

const long long mod=1e9+7;
const int maxn=1e3+5;
const int INF=0x7fffffff;
const LL inf=0x3f3f3f3f;
const double eps=1e-8;
int a[3],b[3];
pair<int,int>p[3];
int x[maxn*100],y[maxn*1000];
int mp[maxn][maxn];
int main() {
    for(int i=0; i<3; i++) {
        scanf("%d%d",&a[i],&b[i]);
        p[i].first=a[i];
        p[i].second=b[i];
    }
    sort(a,a+3);
    sort(b,b+3);
    sort(p,p+3);
    printf("%d\n",a[2]-a[0]+b[2]-b[0]+1);
    int x1=p[0].first,y1=p[0].second,x2=p[1].first,x3=a[2];
//    debug(x1);
//    debug(x2);
//    debug(x3);
    int k=0;
    for(int i=0;i<=x2-x1;k++,i++){
        x[k]=x1+i;
        y[k]=y1;
        mp[x[k]][y[k]]=1;
    }
    for(int i=0;i<=b[2]-b[0];i++){
        if(mp[x2][b[0]+i])continue;
        x[k]=x2;
        y[k]=b[0]+i;
        mp[x[k]][y[k]]=1;
        k++;
    }

    for(int i=1;i<=x3-x2;i++){
        if(mp[x2+i][p[2].second])continue;
//        debug(y[0]);
        x[k]=x2+i;
        y[k]=p[2].second;
        k++;
//        debug(k);
    }
    k=a[2]-a[0]+b[2]-b[0]+1;
    for(int i=0;i<k;i++)
        printf("%d %d\n",x[i],y[i]);
    return 0;
}
```
 

 
# [D. Minimum Diameter Tree](http://codeforces.com/contest/1087/problem/D)

 直径是通过权值分配来搞定的，要直径最大值最小。直径最大值肯定是从每一个叶子走到另一个叶子，所以直接算有多少个叶子，答案就是s*2.0/(叶子数量)。任意两个叶子之间的距离相同，就是最小。

 

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>
#include<bitset>

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> P;

#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));

const long long mod=1e9+7;
const int maxn=1e6+5;
const int INF=0x7fffffff;
const LL inf=0x3f3f3f3f;
const double eps=1e-8;
vector<int> G[maxn];
int pre[maxn],in[maxn];
int k[maxn],u[maxn];
int main() {
    int n;
    double s;
    scanf("%d%lf",&n,&s);
    for(int i=0; i<n-1; i++) {
        int a,b;
        scanf("%d%d",&a,&b);
        in[a]++;
        in[b]++;
        G[a].push_back(b);
        G[b].push_back(a);
    }
    double ans=0;
    for(int i=1; i<=n; i++) {
        if(in[i]==1) {
           ans+=1;
        }
    }
   
    printf("%.10f",s*2.0/ans);
    return 0;
}
```
 

   
 