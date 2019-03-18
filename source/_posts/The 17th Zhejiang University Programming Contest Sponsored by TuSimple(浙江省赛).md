---
title: The 17th Zhejiang University Programming Contest Sponsored by TuSimple(浙江省赛)
date: 2018-08-27 20:46:15
tags: CSDN迁移
---
  ## [A - Marjar Cola](https://vjudge.net/problem/ZOJ-3948)

 [ZOJ - 3948](https://vjudge.net/problem/741684/origin)

 签到题，x,y,a,b,都很小直接暴力。判断INF，只要判断次数有没有过多就行。

 
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

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
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
const int maxn=5e5+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;


int main() {
    int x,y,a,b;
    int t;
    scanf("%d",&t);
    while(t--) {
        int ans=0;
        scanf("%d%d%d%d",&x,&y,&a,&b);
        if(x==1||y==1)puts("INF");
        else {
            while(a>=x||y<=b) {
                if(a>=x) {
                    a-=x;
                    a++;
                    b++;
                    ans++;
                } else {
                    b-=y;
                    b++;
                    a++;
                    ans++;
                }
//                debug(a);
//                debug(b);
                if(ans>3e5){
                    break;
                }
            }
            if(ans>3e5){
                puts("INF");
            }
            else printf("%d\n",ans);
        }
    }
    return 0;
}
```
 
## [C - How Many Nines](https://vjudge.net/problem/ZOJ-3950)

 [ZOJ - 3950 ](https://vjudge.net/problem/741686/origin)

 打个表，比较考虑细节，真的难处理。

 
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

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
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
const int maxn=1e4+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;

int Year[maxn], Mon[35], pre1[maxn], pre2[35];
int hh[13], mon[13];
bool leap(int y){
    if(y%400==0)return 1;
    if(y%100==0)return 0;
    if(y%4==0)return 1;
    return 0;
}
int nine(int y){
    int cnt = 0;
    while(y){
        if(y%10==9)cnt++;
        y/=10;
    }
    return cnt;
}
void init(){
    Mon[1]=Mon[3]=Mon[5]=Mon[7]=Mon[8]=Mon[10]=Mon[12]=3;///31
    Mon[2]=2;
    Mon[4]=Mon[6]=Mon[9]=Mon[11] = 3;///30
    Mon[9] += 30;
    hh[1]=hh[3]=hh[5]=hh[7]=hh[8]=hh[10]=hh[12]=31;
    hh[2] = 28;
    hh[4]=hh[6]=hh[9]=hh[11]=30;
    mon[0] = 0;
    for(int i = 1; i <= 12; ++i){
        mon[i] = hh[i]+mon[i-1];
    }
    int sum = 0;pre1[1999]=pre2[0] = 0;
    for(int i = 1; i <= 12; ++i){
        sum += Mon[i];
        pre2[i] = pre2[i-1]+Mon[i];
    }
    for(int i = 2000; i <= 9999; ++i){
        int tmp = nine(i);
        if(tmp){
            Year[i] = sum + tmp*365;
            if(leap(i))Year[i] += tmp + 1;
        }else{
            Year[i] = sum;
            if(leap(i))++Year[i];
        }
        pre1[i] = pre1[i-1]+Year[i];
    }
}
int get(int d){
    if(d<=9)return 3;
    if(d<=19)return 2;
    if(d<=29)return 1;
    return 0;
}
int main(){
#ifndef ONLINE_JUDGE
    //freopen("E://ADpan//in.in", "r", stdin);
    //freopen("E://ADpan//out.out", "w", stdout);  
#endif
    init();
    int tim;scanf("%d",&tim);
    while(tim--){
        int y1,m1,d1,y2,m2,d2;
        scanf("%d%d%d%d%d%d",&y1,&m1,&d1,&y2,&m2,&d2);
        if(y1==y2){
            if(m1==m2){
                int ans = get(d1)-get(d2+1);
                if(m1==9)ans += d2-d1+1;
                int tmp = nine(y1);
                ans += tmp*(d2-d1+1);
                printf("%d\n", ans);
                continue;
            }
            int ans = pre2[m2-1]-pre2[m1];
            if(leap(y1)&&2>m1&&2<m2)ans++;
            ans += get(d1) + 3 - get(d2+1);
            if(m1==2&&leap(y1)==false)ans--;
            if(m1==9){
                ans+=30-d1+1;
            }
            if(m2==9){
                ans += d2;
            }
            int tmp = nine(y1);
            if(tmp){
                int day = mon[m2-1]-mon[m1] + (hh[m1] - d1 + 1) + d2;
                if(leap(y1)&&2>m1&&2<m2)day++;
                if(leap(y1)&&m1==2)day++;
                ans += day*tmp;
            }
            printf("%d\n", ans);
        }else{
            int ans = pre1[y2-1]-pre1[y1];
            int a = 0, b = 0, tmp = nine(y1),day;
            a = pre2[12]-pre2[m1];
            a += get(d1);
            if(leap(y1)==0&&m1==2)a--;
            if(leap(y1)&&m1==1)a++;
            if(m1==9)a += 30-d1+1;
            b = pre2[m2-1];
            b += 3-get(d2+1);
            if(leap(y2)&&m2>2)b++;
            if(m2==9)b+=d2;
            if(tmp){
                day = mon[12]-mon[m1] + (hh[m1] - d1 + 1);
                if(leap(y1)&&m1<=2)day++;
                a += tmp * day;
            }
            tmp = nine(y2);
            if(tmp){
                day = mon[m2-1] + d2;
                if(leap(y2)&&m2>2)day++;
                b += tmp * day;
            }
            ans += a+b;
            printf("%d\n", ans);
        }
    }
    return 0;
}
```
 
## [F - Knuth-Morris-Pratt Algorithm](https://vjudge.net/problem/ZOJ-3957)

 [ZOJ - 3957 ](https://vjudge.net/problem/741693/origin)

 签到题，KMP，判断一下次数就行。暴力判断也没事

 
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

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
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
const int maxn=5e5+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
int n;
char ch1[]="dog",ch2[]="cat";
int nex[2000];
void get_next(char *t,int lent){
    nex[0] = -1;
    for(int i = 0,k = -1;i < lent;){
        if(k==-1||t[i] == t[k]){
            ++k;++i;
            nex[i]=k;
        }else k = nex[k];
    }
}
int kmp(char *s,int lens,char *t,int lent){
    if(lens<lent)return 0;
    get_next(t,lent);
    int i = 0, j = 0;
    int cnt = 0;
    while(i < lens&&j<lent) {
        if(j==-1||s[i] == t[j]){
            i++;j++;
            if(j==lent){
                cnt++;
                j = nex[j];
            }
        }else j=nex[j];
    }
    return cnt;
}

int main() {
    int t;
    scanf("%d",&t);
    while(t--){
        char s[2000];
        scanf("%s",s);
        int l=strlen(s);
        int ans=0;
        ans+=kmp(s,l,ch1,3);
        ans+=kmp(s,l,ch2,3);
        printf("%d\n",ans);
    }
    return 0;
}
```
 
## [G - Intervals](https://vjudge.net/problem/ZOJ-3953)

 [ZOJ - 3953 ](https://vjudge.net/problem/741689/origin)

 贪心，真的贪的内心崩溃。贪心策略，按L排序，每次判断3个区间，如果出现3个重合，丢去 R最大的，因为影响最大。

 
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

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
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
const int maxn=5e5+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
struct seg {
    int st,en,id;
    bool operator < (const  seg a )const {
        return en<a.en;
    }
} sg[maxn];
bool cmp(seg a,seg b) {
    if(a.st==b.st) {
        return a.en<b.en;
    }
    return a.st<b.st;
}
int res[maxn];


int main() {
    int n,t;
    scanf("%d",&t);
    while(t--) {
        scanf("%d",&n);
        for(int i=0; i<n; i++) {
            scanf("%d%d",&sg[i].st,&sg[i].en);
            sg[i].id=i+1;
        }
        sort(sg,sg+n,cmp);
        priority_queue<seg>q;
        int ans=0;
        for(int i=0; i<n; i++) {
            if(q.size()<2) {
                q.push(sg[i]);
            } else {
                q.push(sg[i]);
                seg s[3];
                for(int i=0; i<3; i++) {
                    s[i]=q.top();
                    q.pop();
                }
                if(s[0].st<=s[2].en&&s[1].st<=s[2].en) {
                    res[ans++]=s[0].id;
                    q.push(s[1]);
                    q.push(s[2]);
                } else {
                    q.push(s[0]);
                    q.push(s[1]);
                }
            }
        }
        sort(res,res+ans);
        printf("%d\n",ans);
        for(int i=0; i<ans; i++) {
            printf("%d",res[i]);
            if(i+1==ans)printf("\n");
            else printf(" ");
        }
        if(ans==0)puts("");
    }
    return 0;
}
```
 
## [H - Seven-Segment Display](https://vjudge.net/problem/ZOJ-3954)

 [ZOJ - 3954 ](https://vjudge.net/problem/741690/origin)

 题意 ：1-9九个数，分别可以用后面7个0 1表示，下面给你n个数，每个数后面跟有7个01串，你可以交换n个数任意两列。如果可以通过交换表示出来输出YES 否则NO。

 例子 :

 
```
 7 0101011 1 1101011
```
 把第2列和第5列交换，变成

 
```
 1 1001111 7 0001111 
```
 与1 7 的表示匹配，所以输出YES

 题解：这题只有9个数，暴力啊匹配，n^2都不会超时。。。；我是把每一列的状态用一个10进制数保存，每次能够从原来的数里面找与之匹配的状态，如果找不到，就输出NO；

 
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

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
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
const int maxn=5e5+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
int mp[10];
int n;
int k[10],v[10];
int p[10];
map<int,int> m;
int main() {
    mp[1]=1001111;
    mp[2]=10010;
    mp[3]=110;
    mp[4]=1001100;
    mp[5]=100100;
    mp[6]=100000;
    mp[7]=1111;
    mp[8]=0;
    mp[9]=100;
    int t;
    p[0]=1;
    for(int i=1; i<10; i++) {
        p[i]=p[i-1]*10;
    }
    scanf("%d",&t);
    while(t--) {
        scanf("%d",&n);
        for(int i=0; i<n; i++) {
            scanf("%d%d",&k[i],&v[i]);
        }
        m.clear();
        for(int i=0; i<7; i++) {
            int sum=0;
            for(int j=0; j<n; j++) {
                sum+=mp[k[j]]/p[i]%10*p[j];
            }
            m[sum]++;
        }
        int flag=1;
        for(int i=0; i<7; i++) {
            int sum=0;
            for(int j=0; j<n; j++) {
                sum+=v[j]/p[i]%10*p[j];
            }
            m[sum]--;
            if(m[sum]<0) {
                flag=0;
                break;
            }
        }
        puts(flag?"YES":"NO");
    }
    return 0;
}
```
 
## [J - Course Selection System](https://vjudge.net/problem/ZOJ-3956)

 [ZOJ - 3956 ](https://vjudge.net/problem/741692/origin)

 傻逼题,一开始还在想怎么推公式，结果发现，CI的和只有5e4，枚举所用能够到的状态H 的和最大不久行了，直接转换成了 0 1 背包。。。。。

 
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

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
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
const int maxn=5e4+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
int n;
int a[maxn];
int x[maxn],y[maxn];
int main() {
    int t;
    scanf("%d",&t);
    while(t--) {
        scanf("%d",&n);
        for(int i=0; i<n; i++) {
            scanf("%d%d",&x[i],&y[i]);
        }
        mem(a,-1);
        a[0]=0;
        for(int i=0; i<n; i++) {
            for(int j=5e4-y[i]; j>=0; j--) {
                if(a[j]!=-1) {
                    a[j+y[i]]=max(a[j+y[i]],a[j]+x[i]);
                }
            }
        }
        ll ans=0;
        for(ll i=1;i<=5e4;i++){
            if(a[i]!=-1)
            ans=max(ans,1LL*a[i]*a[i]-i*a[i]-i*i);
        }
        printf("%lld\n",ans);
    }
    return 0;
}
```
 

   
 