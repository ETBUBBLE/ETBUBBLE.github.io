---
title: 2018-2019 ACM-ICPC, Asia Nanjing Regional Contest M
date: 2019-01-23 17:08:18
tags: CSDN迁移
---
  ## [2018-2019 ACM-ICPC, Asia Nanjing Regional Contest M](http://codeforces.com/gym/101981/attachments)

 扩展KMP+马拉车回文串

 s:ababa

 t:aba

 题意：将第一个字符串的一个字串，与第二个字符串从 (0-k)的字符连在一起可以成为回文字符串，且第一个字符串字串的长度比第二个字符串的长度要大。

 要构成的的回文字符串 两部分构成 s' 第一个字符串的字串，和第二个字符串的前缀t'，构成一个回文字符串。

 那么如果把第一个字符串倒过来， 那就相当于，s' 的一部分是和 t'是相同的，s'还有一部分是回文字符串。

 那么s'与t'相同的长度 * 从当前位置能够产生的回文串数量，就相当能够构成的回文串个数。

 
```
 #include<bits/stdc++.h>
using namespace std;
typedef long long LL;
#define debug(x) cout<<"["<<x<<"]"<<endl;
const int maxn=1e6+5;
int nxt[maxn*2],ex[maxn*2];
void getnext(char * str) {
    int i=0,j,po,len=strlen(str);
    nxt[0]=len;
    while(i+1<len&&str[i]==str[i+1])i++;
    nxt[1]=i;
    po=1,j=0;
    for(i=2; i<len; i++) {
        int p=nxt[po]+po;
        if(nxt[i-po]+i<p) {
            nxt[i]=nxt[i-po];
        } else {
            j=p-i;
            if(j<0)j=0;
            while(i+j<len&&str[j]==str[j+i]) j++;
            nxt[i]=j;
            po=i;
        }
    }
}

void exkmp(char *s1,char *s2) {
    int i=0,j,po,len=strlen(s1),l2=strlen(s2);
    getnext(s2);
    while(s1[i]==s2[i]&&i<l2&&i<len)i++;
    ex[0]=i;
    po=0;
    for(i=1; i<len; i++) {
        int p=ex[po]+po;
        if(nxt[i-po]+i<p) {
            ex[i]=nxt[i-po];
        } else {
            j=max(0,p-i);
            while(i+j<len&&j<l2&&s1[i+j]==s2[j]) {
                j++;
            }
            ex[i]=j;
            po=i;
        }
    }
}

char Ma[maxn*2];
int Mp[maxn*2],pos[maxn*2];
int dp[maxn*2],cnt[maxn];
void Manacher(char s[],int len) {   //一名大佬的写法0.0
    memset(pos,-1,sizeof(pos));
    int l=0;
    Ma[l++]='$';
    Ma[l++]='#';
    for(int i=0; i<len; i++) {
        pos[l]=i;
        Ma[l++]=s[i];
        Ma[l++]='#';
    }
    Ma[l]=0;
    int mx=0,id=0;
    for(int i=0; i<l; i++) {
        Mp[i]=mx>i?std::min(Mp[2*id-i],mx-i):1;
        while(i-Mp[i]>=0&&Ma[i+Mp[i]]==Ma[i-Mp[i]]) Mp[i]++;
        if(i+Mp[i]>mx) {
            mx=i+Mp[i];
            id=i;
        }
    }
    for(int i=0; i<l; i++) {
        dp[i+Mp[i]-1]++;
        if(i>0) dp[i-1]--;
    }
    for(int i=l-1; i>=0; i--) {
        dp[i]+=dp[i+1];
    }
    for(int i=0; i<l; i++) {
        if(pos[i]==-1) continue;
        cnt[pos[i]]=dp[i];
    }
}
char s[maxn],c[maxn];
int main() {
    scanf("%s%s",s,c);
    int len=strlen(s);
    reverse(s,s+len);
    Manacher(s,len);
    exkmp(s,c);
    LL ans=0;
    for(int i=1; i<len; i++) {
        ans+=1LL*cnt[i-1]*ex[i];
    }
    printf("%lld\n",ans);
    return 0;
}

```
 

   
 