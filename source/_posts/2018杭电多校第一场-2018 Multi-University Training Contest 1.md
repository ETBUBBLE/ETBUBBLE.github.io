---
title: 2018杭电多校第一场-2018 Multi-University Training Contest 1
date: 2018-07-23 21:47:53
tags: CSDN迁移
---
  因为去了躺上海，导致两场牛客多校没有打。这场杭电多校在努力也只能写5题，有了大佬讲题解我就过一下。

 
# Maximum Multiple

 **Time Limit: 4000/2000 MS (Java/Others) Memory Limit: 32768/32768 K (Java/Others)  
 Total Submission(s): 0 Accepted Submission(s): 0**  
  
 

 **Problem Description**

 Given an integer n, Chiaki would like to find three positive integers x, y and z such that: n=x+y+z, x∣n, y∣n, z∣n and xyz is maximum.

 

 

 **Input**

 There are multiple test cases. The first line of input contains an integer T (1≤T≤106), indicating the number of test cases. For each test case:  
 The first line contains an integer n (1≤n≤106).

 

 

 **Output**

 For each test case, output an integer denoting the maximum xyz. If there no such integers, output −1 instead.

 

 

 **Sample Input**

 
```
  
```
 3 1 2 3

 

 

 **Sample Output**

 
```
  
```
 -1 -1 1

 

 n/s+n/t+n/k=n;

 求得 s=3 t=3 k=3 /s=2 t=1 k=1;这两种情况最小，然后暴力就行了

 我的写法是打了个表，数据范围不大。

 #1001

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>

using namespace std;
typedef long long ll;
typedef unsigned long long ull;

const long long mod=1e9+7;
const int maxn=1e6+25;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;

int t,n;
long long a[maxn];

int main() {
    memset(a,-1,sizeof(a));
    for(long long i=1;i*4<=maxn;i++){
        a[i*4]=i*i*i*2;
    }
    for(long long i=1;i*3<=maxn;i++){
        a[i*3]=i*i*i;
    }
    cin>>t;
    while(t--) {
        scanf("%d",&n);
        printf("%lld\n",a[n]);
    }
    return 0;
}
```
 #1002

 
# Balanced Sequence

 **Time Limit: 2000/1000 MS (Java/Others) Memory Limit: 32768/32768 K (Java/Others)  
 Total Submission(s): 0 Accepted Submission(s): 0**  
  
 

 **Problem Description**

 Chiaki has n strings s1,s2,…,sn consisting of '(' and ')'. A string of this type is said to be balanced:  
  
 + if it is the empty string  
 + if A and B are balanced, AB is balanced,  
 + if A is balanced, (A) is balanced.  
  
 Chiaki can reorder the strings and then concatenate them get a new string t. Let f(t) be the length of the longest balanced subsequence (not necessary continuous) of t. Chiaki would like to know the maximum value of f(t) for all possible t.

 

 

 **Input**

 There are multiple test cases. The first line of input contains an integer T, indicating the number of test cases. For each test case:  
 The first line contains an integer n (1≤n≤105) -- the number of strings.  
 Each of the next n lines contains a string si (1≤|si|≤105) consisting of `(' and `)'.  
 It is guaranteed that the sum of all |si| does not exceeds 5×106.

 

 

 **Output**

 For each test case, output an integer denoting the answer.

 

 

 **Sample Input**

 
```
  
```
 2 1 )()(()( 2 ) )(

 

 

 **Sample Output**

 
```
  
```
 4 2

 

 先处理字符串

 把他简化最后所有字符串都会变成

 )))(((

 像这样的形式。

 然后贪心一下每次选左边最长或者右边最长，都没有影响，反正是往两边添加，只要保证选的这个是一种的最大的就行。

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int maxn=1e5+25;
int t,n;
stack<char> s;
char ch[maxn];
int dp1[maxn],dp2[maxn],ans;
struct three {
    int d1,d2,n;
} d[maxn];

bool cmp(three &a,three &b){
    if(a.d1==b.d1){
        return a.d2>b.d2;
    }
    return a.d1>b.d1;
}

int main() {
    scanf("%d",&t);
    while(t--) {
        ans=0;
        scanf("%d",&n);
        for(int i=0; i<n; i++) {
            scanf("%s",ch);
            int pos=0;
            while(ch[pos]!='\0') {
                if(ch[pos]==')') {
                    if(s.size()==0||s.top()==')') {
                        s.push(')');
                    } else {
                        ans+=2;
                        s.pop();
                    }
                } else if(ch[pos]=='(') {
                    s.push('(');
                }
                pos++;
            }
            int a1=0,a2=0;
            if(s.size()==0) {
                i--;
                n--;
            } else {
                while(s.size()>0) {
                    if(s.top()==')')a2++;
                    else a1++;
                    s.pop();
                }
                d[i].d1=a1;
                d[i].d2=a2;
                d[i].n=i;
            }
        }
        sort(d,d+n,cmp);
        int k1=d[0].d1,k2=d[0].d2;
        for(int i=1; i<n; i++) {
            int k=max(min(k1,d[i].d2),min(k2,d[i].d1));
            ans+=2*k;

            k1=k1-k+d[i].d1;
            k2=k2-k+d[i].d2;
        }
        printf("%d\n",ans);
    }
    return 0;
}
```
 #1003

 
# Triangle Partition

 **Time Limit: 2000/1000 MS (Java/Others) Memory Limit: 132768/132768 K (Java/Others)  
 Total Submission(s): 0 Accepted Submission(s): 0  
Special Judge**  
 

 **Problem Description**

 Chiaki has 3n points p1,p2,…,p3n. It is guaranteed that no three points are collinear.  
 Chiaki would like to construct n disjoint triangles where each vertex comes from the 3n points.

 

 

 **Input**

 There are multiple test cases. The first line of input contains an integer T, indicating the number of test cases. For each test case:  
 The first line contains an integer n (1≤n≤1000) -- the number of triangle to construct.  
 Each of the next 3n lines contains two integers xi and yi (−109≤xi,yi≤109).  
 It is guaranteed that the sum of all n does not exceed 10000.

 

 

 **Output**

 For each test case, output n lines contain three integers ai,bi,ci (1≤ai,bi,ci≤3n) each denoting the indices of points the i-th triangle use. If there are multiple solutions, you can output any of them.

 

 

 **Sample Input**

 
```
  
```
 1 1 1 2 2 3 3 5

 

 

 **Sample Output**

 
```
 1 2 3
```
 没啥好讲的，排个序从左到右反正不会交叉。

 
```
 #include <cstdio>
#include <string>
#include <cstring>
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;

typedef long long LL;
typedef pair<char, int > PCI;
typedef pair<int, int> PII;
typedef pair<LL, LL> PLL;
const int MAX = 1e4+7;
const int INF = 0x3f3f3f3f;
const int mod=1e9+7;
int N, M, K, T;

struct node {
    int x, y, id;
    bool operator<(const node& b) const {
        return x < b.x;
    }
};
node a[MAX];
vector<int> v[MAX];

int main() {
    
    scanf("%d",&T);
    while(T--) {
        scanf("%d",&N);
        for(int i = 1; i <= 3*N; i++) {
            scanf("%d %d", &a[i].x, &a[i].y);
            a[i].id = i;
        }
        sort(a+1, a+N*3+1);
        for(int i = 1; i <= 3*N; i+=3) {
            printf("%d %d %d\n",a[i].id, a[i+1].id, a[i+2].id);
        }
    }


    return 0;
}
```
 #1004

 hiaki has an array of n positive integers. You are told some facts about the array: for every two elements ai and aj in the subarray al..r (l≤i<j≤r), ai≠ajholds.  
 Chiaki would like to find a lexicographically minimal array which meets the facts.

 

 

 Input

 There are multiple test cases. The first line of input contains an integer T, indicating the number of test cases. For each test case:  
  
 The first line contains two integers n and m (1≤n,m≤105) -- the length of the array and the number of facts. Each of the next m lines contains two integers li and ri (1≤li≤ri≤n).  
  
 It is guaranteed that neither the sum of all n nor the sum of all m exceeds 106.

 

 

 Output

 For each test case, output n integers denoting the lexicographically minimal array. Integers should be separated by a single space, and no extra spaces are allowed at the end of lines.

 

 

 Sample Input

 
```
  
```
 3 2 1 1 2 4 2 1 2 3 4 5 2 1 3 2 4

 

 

 Sample Output

 
```
  
```
 1 2 1 2 1 2 1 2 3 1 1

 

 队友写的，看了一下，贪心，每次放最小的就行。

 
```
 #include <bits/stdc++.h>
#define fi first
#define se second
#define lson l,m,rt<<1
#define rson m+1,r,rt<<1|1
#define lowbit(x) x&-x
#define MP make_pair
#define debug(x) cout<<x<<"= "<<x<<endl;
#define FIN freopen("in.txt","r",stdin);
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int>pii;
typedef pair<ll,ll>pll;
const int mod=1e9+7;
const int inf=0x3f3f3f3f;
const ll infll=0x3f3f3f3f3f3f3f3f;
const int MX=1e5+7;

int n,m;
struct node {
    int l,r;

    bool operator<(const node&A)const {
        if(l==A.l)
            return r<A.r;
        return l<A.l;
    }
} a[MX];
int ans[MX];
bool vis[MX];

int main() {
    int T;
    cin>>T;
    while(T--) {
        scanf("%d%d",&n,&m);
        for(int i=1; i<=m; i++) {
            scanf("%d%d",&a[i].l,&a[i].r);
        }
        sort(a+1,a+m+1);
        priority_queue<int,vector<int>,greater<int> >q;
        for(int i=1; i<=n; i++){
            ans[i]=1;vis[i]=0;
            q.push(i);
        }
        int l=a[1].l,r=a[1].l;
        for(int i=1;i<=m;i++){
            for(;l<a[i].l;l++){
                if(vis[l]) q.push(ans[l]);
            }
            for(;r<=a[i].r;r++){
                if(r>=a[i].l){
                    ans[r]=q.top();q.pop();
                    vis[r]=1;
                }
            }
        }
        for(int i=1; i<=n; i++) {
            printf("%d",ans[i]);
            if(i==n) printf("\n");
            else printf(" ");
        }
    }
    return 0;
}
```
 #1007

 
# Chiaki Sequence Revisited

 ****Time Limit: 2000/1000 MS (Java/Others) Memory Limit: 32768/32768 K (Java/Others)  
 Total Submission(s): 428 Accepted Submission(s): 69****  
  
 

 **Problem Description**

 Chiaki is interested in an infinite sequence a1,a2,a3,..., which is defined as follows: 

 an={1an−an−1+an−1−an−2n=1,2n≥3

 Chiaki would like to know the sum of the first n terms of the sequence, i.e. ∑i=1nai. As this number may be very large, Chiaki is only interested in its remainder modulo (109+7).

 

 

 **Input**

 There are multiple test cases. The first line of input contains an integer T (1≤T≤105), indicating the number of test cases. For each test case:  
 The first line contains an integer n (1≤n≤1018).

 

 

 **Output**

 For each test case, output an integer denoting the answer.

 

 

 **Sample Input**

 
```
  
```
 10 1 2 3 4 5 6 7 8 9 10

 

 

 **Sample Output**

 
```
  
```
 1 2 4 6 9 13 17 21 26 32

 打个表找规律，

 1 2 3 4 5 6 7 8 9 10

 2 2 1 3 1 2 1 4 1 2

 各个数出现的次数就是这样，然后就是lowbit（i）次，然后二分找 到哪个数出现的数次数总和为n

 然后再看一下 每次找的的数，x,你会发现，x总是在n/2附近。

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>
#include<bitset>
#include <complex>
#include <functional>

using namespace std;
typedef long long ll;
typedef unsigned long long ull;

const long long mod=1e9+7;
const int maxn=1e7+25;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;

long long m,n;
long long sum(ll x) {
    ll ans=1,k=1;
    while(x>0) {
//        cout<<x<<endl;
        if(x%2==1)ans+=x/2*k+1*k;
        else {
            ans+=x/2*k;
        }
        k++;
        x/=2;
    }
    return ans;
}
long long js(ll x) {
    ll res=0,k=1,ans,p=1;
    while(x>0) {
        if(x%2==1) {
            ans=x/2+1;
        } else {
            ans=x/2;
        }
        ans%=mod;
        res+=ans*ans%mod*p%mod*k%mod;
        res%=mod;
        k++;
        p=p*2%mod;
        x/=2;
    }
    return res%mod;
}
int main() {
    int t;
    cin>>t;
    while(t--) {
        scanf("%lld",&n);
        ll l=0,r=n;
        if(n>200){
            l=n/2-100;
            r=n/2+100;
        }
        while(l<r-1) {
            ll mid=(l+r)/2;
            if(sum(mid)>n) {
                r=mid;
            } else l=mid;
        }
//        cout<<l<<endl;
        printf("%lld\n",(js(l)+(n-sum(l))*(l+1)%mod+1)%mod);
    }
    return 0;
}
```
 #1011

 
# Time Zone

 **Time Limit: 2000/1000 MS (Java/Others) Memory Limit: 32768/32768 K (Java/Others)  
 Total Submission(s): 1646 Accepted Submission(s): 289**  
  
 

 **Problem Description**

 Chiaki often participates in international competitive programming contests. The time zone becomes a big problem.  
 Given a time in Beijing time (UTC +8), Chiaki would like to know the time in another time zone s.

 

 

 **Input**

 There are multiple test cases. The first line of input contains an integer T (1≤T≤106), indicating the number of test cases. For each test case:  
 The first line contains two integers a, b (0≤a≤23,0≤b≤59) and a string s in the format of "UTC+X'', "UTC-X'', "UTC+X.Y'', or "UTC-X.Y'' (0≤X,X.Y≤14,0≤Y≤9).

 

 

 **Output**

 For each test, output the time in the format of hh:mm (24-hour clock).

 

 

 **Sample Input**

 
```
  
```
 3 11 11 UTC+8 11 12 UTC+9 11 23 UTC+0

 

 

 **Sample Output**

 
```
  
```
 11:11 12:12 03:23

 注意精度

 
```
 include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>

using namespace std;
typedef long long ll;
typedef unsigned long long ull;

const long long mod=1e9+7;
const int maxn=1e7+25;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;

int t;

int main() {
    cin>>t;
    int h,m;
    double ut;
    while(t--) {
        scanf("%d%d UTC%lf",&h,&m,&ut);
        int u=ut*100;
        int k=abs(u);
        if(k%10>5)k=k/10+1;
        else k=k/10;
        if(u>0)u=k;
        else u=-k;
//        cout<<u<<endl;
        m+=u%10*6;
        if(m<0) {
            m+=60;
            h--;
        }
        h+=m/60;
        m=m%60;
//        cout<<h<<endl;
        m=m%60;
        u=u/10-8;
        h+=u;
        if(h<0)h+=24;
        h=h%24;
        printf("%02d:%02d\n",h,m);
    }
    return 0;
}
```
 

   
 