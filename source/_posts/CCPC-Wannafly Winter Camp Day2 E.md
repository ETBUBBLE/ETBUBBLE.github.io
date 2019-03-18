---
title: CCPC-Wannafly Winter Camp Day2 E
date: 2019-02-01 00:18:47
tags: CSDN迁移
---
  # [Power of Function](https://www.zhixincode.com/contest/10/problem/E?problem_id=150)

 这个题重点是读题。看懂函数，函数最终表示的是 n 写成K进制，K进制的值的和加上长度 -2 就是m。

 给你一个 ，k,l,r,求K进制下，[l,r]区间内m的最大值。然后输出当m最大时[l,r]区间最大值和最小值。

 

 题解：把l,r转换成K进制， r>l，如果r,l,高位相同，那么求得到最大值M高位肯定也是和这个值一样。然后从第一个位不同开始，想让M值最大，只有两种可能，一种是取r这个值二进制下位减一，然后后面位的全部取 k-1,或者这个位取最大值再继续讨论下一个位。写个DFS就可以，和数位DP有点像。

 举个例子 ：k=10 ,l=1001,r=1179.

 10进制下 l = 1 0 0 1

 r = 1 1 7 9

 最前面 1 和 1 是相同的所以要 m最大 肯定 最高位也是 1 b=1 0 0 0

 然后从第3位开始dfs(3) 要么这个位取 1， 要么 取 0 后慢慢全取 9，

 如果这个位 取 1 就会影响下一个位，所以再DFS（2） 然后发现 要么取7 ，（要么 取 6 后面全为9）

 以此类推，最后发现 后面3为 179 099 取得 后面比较大，那么m取最大就是 1099；

 还有一些细节要注意，比如 179 ，99是一样打的，因为99只有两位数。还有 0 是没有 比他小1这个数，如果要小1就要向高位借，所以这情况要排除。

 
```
 #include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
int t;
LL k,l,r;
vector<LL> v1,v2,mi,mx;
LL p[2000];
LL dfs(LL pos,bool limit) {
    if(pos==-1)return 0;
    if(limit) {
        LL m1,m2;
        m1=dfs(pos-1,1);
        m2=dfs(pos-1,0);
        m1+=v2[pos];
        m2+=v2[pos]-1;
        if(v2[pos]==0)m2=-1;    //排除 为 r pos位 为0情况 
        if(pos==v2.size()-1&&v2[pos]==1)m2--;  // 最高位为 0 m2要减一
        if(m1>m2) {           
            mx[pos]=mi[pos]=v2[pos];
            return m1;
        } else if(m1==m2) {
            mx[pos]=v2[pos];
            mi[pos]=v2[pos]-1;
            for(int i=0; i<pos; i++) {
                mi[i]=k-1;
            }
            return m2;
        } else {
            mx[pos]=mi[pos]=v2[pos]-1;
            for(int i=0; i<pos; i++) {
                mi[i]=mx[i]=k-1;
            }
            return m2;
        }

    } else {
        return (k-1)*(pos+1);
    }
}
int cas=1;
int main() {
    scanf("%d",&t);
    while(t--) {
        scanf("%lld%lld%lld",&k,&l,&r);
        v1.clear();
        v2.clear();
        LL x=r;
        while(x>0) {
            v2.push_back(x%k);
            x/=k;
        }
        x=l;
        while(x>0) {
            v1.push_back(x%k);
            x/=k;
        }
        while(v1.size()<v2.size()) {
            v1.push_back(0);
        }
        p[0]=1;
        for(int i=1; i<v2.size(); i++) {
            p[i]=p[i-1]*k;
        }
        int pos=v2.size()-1;
        mi.resize(v2.size());
        mx.resize(v2.size());
        while(pos>=0) {
            if(v1[pos]==v2[pos]) {
                mi[pos]=mx[pos]=v2[pos];
            } else break;
            pos--;
        }
        if(pos!=-1)dfs(pos,1);
        LL a=0,b=0,m=0,flag=1;
        for(int i=mx.size()-1; i>=0; i--) {
            m+=mx[i];
            if(mx[i]!=0)flag=0;
            if(flag)m--;
            b+=mx[i]*p[i];
            a+=mi[i]*p[i];
        }
        m=m+mx.size()-2;
        printf("Case #%d: %lld %lld %lld\n",cas++,m,a,b);
    }
    return 0;    //看不懂留言0.0，或者加Q3035536707
}

```
 

   
 