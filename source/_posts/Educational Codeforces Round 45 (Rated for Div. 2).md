---
title: Educational Codeforces Round 45 (Rated for Div. 2)
date: 2018-06-10 22:03:38
tags: CSDN迁移
---
  A ,B 两题就直接给代码了，没啥讲的

 A:

 
```
 #include<bits/stdc++.h>
using namespace std;
typedef long long ll;
using LL = long long;
ll n,m,a,b;
int main()
{
  cin>>n>>m>>a>>b;
  ll k=n/m;
  if(n%m==0)
  {
      cout<<0<<endl;
  }
  else {
    cout<<min((n-k*m)*b,((k+1)*m-n)*a)<<endl;
  }
  return 0;
}
```
 

 B:

 

 
```
 #include<bits/stdc++.h>
using namespace std;
typedef long long ll;
using LL = long long;
const int maxn=2e5+25;
int n,a[maxn],k;

int main() {
    cin>>n>>k;
    for(int i=0; i<n; i++) {
        scanf("%d", &a[i]);
    }
    sort(a, a+n);
    int mx=1e7+25,ans=0;;
    for(int i=n-2;i>=0;i--)
    {
        int *temp=upper_bound(a,a+n,a[i]);
        if(temp==a+n)ans++;
        else if(a[i]+k<*temp||a[i]==*temp)ans++;
    }
    cout<<ans+1<<endl;;
    return 0;
}
```
 

 

 [http://codeforces.com/contest/990/problem/C](http://codeforces.com/contest/990/problem/C)

 

 括号匹配。每个字符串保留前缀和

 例如 ((() 前缀和 2

 ())) 前缀和 -2；

 每次只要把两个数前缀和加起来等于0的数量想成就是可以匹配的数量。

 

 保留负数前缀和的时候一定要是这个前缀和的时候一定是最小的那个，不然本身就是错的。

 例如 )))(( ())(

 

 
```
  #include<bits/stdc++.h>
using namespace std;
typedef long long ll;
using LL = long long;
const int maxn=3e5+25;
ll n,mx=0;
char ch[maxn];
map<ll,ll> mp;
int main() {
    cin>>n;
    for(ll j=0; j<n; j++) {
        scanf("%s",ch);
        ll flag=0,k=0,l=strlen(ch);
        for(ll i=0; i<l; i++) {
            if(ch[i]=='(')k++;
            else k--;
            if(k<0)flag=min(k,flag);
        }
        if(flag<0&&k>flag)continue;
        else {
            mp[k]++;
            mx=max(k,mx);
        }
    }
    ll sum=0;
    for(int i=0; i<=mx; i++)
        sum+=mp[i]*mp[-i];
    cout<<sum<<endl;
    return 0;
}
```
 D:[http://codeforces.com/contest/990/problem/D](http://codeforces.com/contest/990/problem/D)

 题目意思是，给你N个顶点，然后怎么连让他可以有，a,个联通快，然后连的的矩阵的反矩阵 刚好有b个联通快。

 例如 3 1 2

 矩阵 是 

 001

 001

 110

 他就是 这个样子 ![](https://img-blog.csdn.net/20180611124117642?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 反矩阵就是

 010

 100

 000

 图就是 ![](https://img-blog.csdn.net/20180611124217482?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 

 这个样子所以满足条件。

 

 看起来挺难的，实际上就是个傻逼题，就是没连的边一定可以连上，所以无论你一种连成啥样另一种必然是全部联通

 所以，a,b必须有一个是 1 ，如果没有就不可行，然后特判一下 2 3 两个 都是 1的情况，为什么要特判呢，自己画个图试试就知道了。

 然后就简单了，矩阵其实只要吧 不是 1 的的那个联通快数量分成 1 1 1 1 n-a 这样的几个联通块就行了。

 所以只要连 n-a条边。

 

 
```
 #include <bits/stdc++.h>
using namespace std;

const int Maxn = 1005;

int n, a, b;
char B[Maxn][Maxn];

int main() {
    scanf("%d%d%d", &n, &a, &b);
    if(a!=1&&b!=1)printf("NO\n");
    else if((n==2||n==3)&&a==1&&b==1)printf("NO\n");
    else {
        char ca='1',cb='0';
        if(a<b) {
            swap(a,b);
            swap(ca,cb);
        }
        int k=n-a;
        for(int i=0; i<n; i++) {
            for(int j=0; j<n; j++) {
                if(i==j)B[i][j]='0';
                else B[i][j]=cb;
            }
        }
        for(int i=0; i<k; i++) {
            B[i][i+1]=B[i+1][i]=ca;
        }
        printf("YES\n");
        for(int i=0; i<n; i++) {
            for(int j=0; j<n; j++)
                printf("%c",B[i][j]);
            printf("\n");
        }
    }

    return 0;
}

```
 

 

 

 

   
 