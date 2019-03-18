---
title: 2019 CCPC-Wannafly Winter Camp Day2（Div2）
date: 2019-01-28 16:05:24
tags: CSDN迁移
---
  Camp 的题是真的难，还好没去，不然要被血虐。

 做了Day2的几道题。

 
# [A题，Erase Numbers II](https://www.zhixincode.com/contest/10/problem/A?problem_id=146)

 这个挺简单的，就是范围炸了long long ,暴力枚举两个数就行了。

 
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
//typedef long long LL;
typedef unsigned long long LL;
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
const int maxn=6e3+5;
const int INF=0x7fffffff;
const LL inf=0x3f3f3f3f;
const double eps=1e-8;

int t,n;
LL a[maxn],m1=0,ans1,ans2;
LL k(LL x,LL y){
    LL k=x;
    while(k>0){
        k/=10;
        y*=10;
    }
    return y+x;
}
int main() {
    scanf("%d",&t);
    int cas=1;
    while(t--) {
        scanf("%d",&n);
        m1=0;
        for(int i=0; i<n; i++) {
            cin>>a[i];
        }
        ans1=ans2=0;
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++)ans1=max(ans1,k(a[j],a[i]));
        }
        printf("Case #%d: ",cas++);
        cout<<ans1<<endl;
    }
    return 0;
}

```
 

 
# [B题：Erase Numbers I](https://www.zhixincode.com/contest/10/problem/B?problem_id=147)

 题意：删除两个数，最后连起来的数字结果最大。

 这题暴力过了，实际上暴力是过不了，本来要预处理一下，删除一个长度为 L的数字串，第一个不同的数字的位置，

 比如：12 55 58 

 删除第二个 字符串的时候， 会变成 1258，和原字符串 125558 字符是 5 和8 不同（这个地方只是举例说明删一个字符串） 

 但是出题人好像没有特意卡，就直接暴力过了

 
```
 #include<bits/stdc++.h>
using namespace std;
typedef long long LL;
//#define debug(x) cout<<"["<<x<<"]"<<endl;
const int maxn=6e3+5;

int a[maxn],b[maxn],t,n,mi,a1,a2;
char s[maxn*10];
char c[maxn][15];
bool pd(int pos) {
    int pos2=pos+1,i=0,j=0; // 就是这个地方要本来是要预处理的，但是暴力每次比较也可以过
    while(1) {
        if(c[pos][i]<c[pos2][j]) {
            return 1;
        } else if(c[pos][i]>c[pos2][j]) {
            return 0;
        }
        i++;
        j++;
        if(i==b[pos]) {
            pos++;
            i=0;
        }
        if(j==b[pos2]) {
            pos2++;
            j=0;
        }
        if(pos2==n)return 1;
    }
}
bool pd2(int pos) {
    int pos2=pos+1,i=0,j=0;
    while(1) {
        if(pos==a1)pos++;
        if(pos2==a1)pos2++;
        if(c[pos][i]<c[pos2][j]) {
            return 1;
        } else if(c[pos][i]>c[pos2][j]) {
            return 0;
        }
        i++;
        j++;
        if(i==b[pos]) {
            pos++;
            i=0;
        }
        if(j==b[pos2]) {
            pos2++;
            j=0;
        }
        if(pos2==n)return 1;
    }
}
int main() {
    scanf("%d",&t);
    int cas=1;
    while(t--) {
        scanf("%d",&n);
        mi=100;
        for(int i=0; i<n; i++) {
            scanf("%s",c[i]);
            b[i]=strlen(c[i]);
            a[i]=0;
            for(int j=0; j<b[i]; j++) {
                a[i]=a[i]*10+c[i][j]-'0';
            }
            mi=min(mi,b[i]);
        }
        a1=-1;
        for(int i=0; i<n; i++) {
            if(b[i]==mi) {
                if(pd(i)) {
                    a1=i;
                    break;
                }
            }
        }
        if(a1==-1) {
            for(int i=n-1; i>=0; i--) {
                if(b[i]==mi) {
                    a1=i;
                    break;
                }
            }
        }
        mi=100;
        a2=-1;
        for(int i=0; i<n; i++) {
            if(i==a1)continue;
            mi=min(b[i],mi);
        }
        for(int i=0; i<n; i++) {
            if(i==a1)continue;;
            if(b[i]==mi) {
                if(pd2(i)) {
                    a2=i;
                    break;
                }
            }
        }
        if(a2==-1) {
            for(int i=n-1; i>=0; i--) {
                if(i==a1)continue;
                if(b[i]==mi) {
                    a2=i;
                    break;
                }
            }
        }
        printf("Case #%d: ",cas++);
        for(int i=0; i<n; i++) {
            if(i==a1||a2==i)continue;
            printf("%d",a[i]);
        }
        puts("");
    }
    return 0;
}
```
 
# [H题：Cosmic Cleaner](https://www.zhixincode.com/contest/10/problem/H?problem_id=153)

 题意：问删除的球和原来给的这些球相交的体积有多少。

 贴个求球相交的体积板子就行了0.0.

 
```
 #include<cstdio>
#include<algorithm>
#include<cstring>
#include<iostream>
#define CLR(a,b) memset(a,b,sizeof(a));
using namespace std;
const double PI = acos(-1);
const int maxn= 105;
typedef struct point {
    double x,y,z;
    point() {
    }
    point(double a, double b,double c) {
        x = a;
        y = b;
        z = c;
    }
    point operator -(const point &b)const {     //返回减去后的新点
        return point(x - b.x, y - b.y,z-b.z);
    }
    point operator +(const point &b)const {     //返回加上后的新点
        return point(x + b.x, y + b.y,z+b.z);
    }
    //数乘计算
    point operator *(const double &k)const {    //返回相乘后的新点
        return point(x * k, y * k,z*k);
    }
    point operator /(const double &k)const {    //返回相除后的新点
        return point(x / k, y / k,z/k);
    }
    double operator *(const point &b)const {    //点乘
        return x*b.x + y*b.y+z*b.z;
    }
} point;

double dist(point p1, point p2) {       //返回平面上两点距离
    return sqrt((p1 - p2)*(p1 - p2));
}
typedef struct sphere {//球
    double r;
    point centre;
} sphere;
sphere s,a[maxn];
void SphereInterVS(sphere a, sphere b,double &v,double &s) {
    double d = dist(a.centre, b.centre);//球心距
    double t = (d*d + a.r*a.r - b.r*b.r) / (2.0 * d);//
    double h = sqrt((a.r*a.r) - (t*t)) * 2;//h1=h2，球冠的高
    double angle_a = 2 * acos((a.r*a.r + d*d - b.r*b.r) / (2.0 * a.r*d));  //余弦公式计算r1对应圆心角，弧度
    double angle_b = 2 * acos((b.r*b.r + d*d - a.r*a.r) / (2.0 * b.r*d));  //余弦公式计算r2对应圆心角，弧度
    double l1 = ((a.r*a.r - b.r*b.r) / d + d) / 2;
    double l2 = d - l1;
    double x1 = a.r - l1, x2 = b.r - l2;//分别为两个球缺的高度
    double v1 = PI*x1*x1*(a.r - x1 / 3);//相交部分r1圆所对应的球缺部分体积
    double v2 = PI*x2*x2*(b.r - x2 / 3);//相交部分r2圆所对应的球缺部分体积
    v = v1 + v2;//相交部分体积
    double s1 = PI*a.r*x1;  //r1对应球冠表面积
    double s2 = PI*a.r*x2;  //r2对应球冠表面积
    s = 4 * PI*(a.r*a.r + b.r*b.r) - s1 - s2;//剩余部分表面积
}
int t, n;
double x, y, z, r;
int cas = 1;
int main() {
    cin >> t;
    while(t--) {
        cin >> n;
        for(int i = 1; i <= n; i++) {
            scanf("%lf%lf%lf%lf",&x,&y,&z,&a[i].r); //其他球
            a[i].centre = {x,y,z};
        }
        scanf("%lf%lf%lf%lf",&x,&y,&z,&r);
        s.r = r;
        s.centre = {x,y,z}; //中心球
        double ans = 0, v = 0;
        for(int i = 1; i <= n; i++) {
            double ss, dis = dist(s.centre, a[i].centre);

            if(dis >= s.r + a[i].r)continue;  //在外部
            if(dis + min(s.r, a[i].r) <= max(s.r, a[i].r)) { //在内部
                ans += 4.0 / 3.0 * PI * min(s.r,a[i].r) * min(s.r,a[i].r) * min(s.r,a[i].r);
                continue;
            }
            SphereInterVS(s, a[i], v, ss); //相交部分
            ans += v;
        }
        printf("Case #%d: %.14f\n",cas++,ans);
    }
}
//搜索板子，搜到的一名大佬的，我改都没改就直接过了0.0
```
 
# [K题：Sticks](https://www.zhixincode.com/contest/10/problem/K)

 预处理出所有分组情况，一共是 C(3,12)*C(3,9)*C(3,6) /24种。 15400种

 让后暴力求解 15400*6000竟然判断过了0.0

 题目没说要怎么输出答案，然后我用SET 错了，用vector 过了。。。。。。

 
```
 #include<bits/stdc++.h>
using namespace std;
typedef long long LL;
#define debug(x) cout<<"["<<x<<"]"<<endl;
const int maxn=369605;
int a[20];
struct th {
    short x[4][3];
    bool operator<(const th& t) const {
    }
} dat,pa[maxn];
int u[20],k=0;

void dfs(int p) {
    if(p==4) {
        pa[k]=dat;
        k++;
        return ;
    }
    for(int i=0; i<12; i++) {
        if(u[i])continue;
        if(dat.x[p-1][0]>i)continue;   //这个是去重的，会有重复的情况
        for(int i2=i+1; i2<12; i2++) {
            if(u[i2])continue;
            for(int i3=i2+1; i3<12; i3++) {
                if(u[i3])continue;
                dat.x[p][0]=i;
                dat.x[p][1]=i2;
                dat.x[p][2]=i3;
                u[i]=u[i2]=u[i3]=1;
                dfs(p+1);
                u[i]=u[i2]=u[i3]=0;
            }
        }
    }
}
int t,cas=1;
int main() {
    dfs(0);
    debug(k);
    scanf("%d",&t);
    while(t--) {
        for(int i=0; i<12; i++)
            scanf("%d",&a[i]);
        int num=0,ans;
        for(int i=0;i<k;i++) {
            int tnum=0;
            for(int j=0; j<4; j++) {
                int m1=a[pa[i].x[j][0]],m2=a[pa[i].x[j][1]],m3=a[pa[i].x[j][2]];
                if(m1+m2>m3&&m2+m3>m1&&m3+m1>m2)tnum++;
            }
            if(tnum>num) {
                num=tnum;
                ans=i;
            }
            if(num==4)break;
        }
        printf("Case #%d: %d\n",cas++,num);
        if(num==0)continue;
        for(int j=0; j<4; j++) {
            int m1=a[pa[ans].x[j][0]],m2=a[pa[ans].x[j][1]],m3=a[pa[ans].x[j][2]];
            if(m1+m2>m3&&m2+m3>m1&&m3+m1>m2){
                printf("%d %d %d\n",m1,m2,m3);
            }
        }
    }
    return 0;
}

```
 

   
 