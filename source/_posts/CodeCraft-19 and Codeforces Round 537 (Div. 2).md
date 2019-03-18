---
title: CodeCraft-19 and Codeforces Round 537 (Div. 2)
date: 2019-02-04 11:09:56
tags: CSDN迁移
---
  对于这一场我是内心崩溃的0.0

 
# [A. Superhero Transformation](http://codeforces.com/contest/1111/problem/A)

 我特么醉了，没任何难度，但是我数组开小了，少打了一个0.。。。。。。。。。。。。。。被fst.

 
```
 #include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
const int maxn=1e7+5;
int k[1000];
int main() {
    k['a']=1;
    k['e']=1;
    k['i']=1;
    k['o']=1;
    k['u']=1;

    char s[2000],t[2000];
    cin>>s>>t;
    int l=strlen(s),l2=strlen(t),flag=1;
    if(l==l2) {
        for(int i=0; i<l; i++) {
            if(k[s[i]]!=k[t[i]])flag=0;
        }
    }
    else flag=0;
    puts(flag?"YES":"NO");
    return 0;
}
```
 
# [B. Average Superhero Gang Power](http://codeforces.com/contest/1111/problem/B)

 直接算就行了，然而我还是错了。直接枚举删除最小的 m个。

 
```
 #include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
const int maxn=1e5+5;
int kk[maxn];
int main() {
    LL n,k,m;
    LL ans=0;
    cin>>n>>k>>m;
    for(int i=0; i<n; i++) {
        scanf("%d",&kk[i]);
        ans+=kk[i];
    }
    sort(kk,kk+n);
    double res=0;
    for(int i=0; i<=min(m,n-1); i++) {
        res=max(res,(double)(min((n-i)*k,m-i)+ans)/(n-i));
        ans-=kk[i];
    }
    printf("%.10f\n",res);
    return 0;
}
```
 
# [C. Creative Snap](http://codeforces.com/contest/1111/problem/C)

 这题好了，终于没有fst了，然而这题才是最崩溃的，一开始就想到了dfs,想了一下复杂度不行，不行你妹啊，然后发现可以，然后计算在区间 [l,r]之间有多少个数，我第一个想到了暴力，我特么想把自己给拍死，二分不行吗，二分不行吗？？？？？我得回到今天凌晨去把自己拍死。

 
```
 #include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> p;
#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
const int maxn=1e5+5;
int n,k,a,b;
LL d[maxn];
LL dfs(LL l, LL r,LL i,LL j,LL cnt) {
    if(cnt<=0) {
        return a;
    } else {
        LL mid=(l+r)/2;
        LL a1=cnt*(r-l+1)*b;
        LL pos=upper_bound(d+i,d+j+1,mid)-d;
        if(r-l==0)return a1;
        return min(a1,dfs(l,mid,i,pos-1,pos-i)+dfs(mid+1,r,pos,j,j-pos+1));
    }
}
int main() {
    scanf("%d%d%d%d",&n,&k,&a,&b);
    for(int i=0; i<k; i++) {
        scanf("%d",&d[i]);
    }
    sort(d,d+k);
    LL ans=0;
    ans=dfs(1LL,1<<n,0LL,k-1,k);
    printf("%lld\n",ans);
    return 0;
}
```
 最终掉分。。。。。。。。。原谅我的菜。

   
 