---
title: KMP
date: 2018-05-23 16:07:45
tags: CSDN迁移
---
  KMP算法，刚接触到这个算法本来一看是看一眼就会了，但是过了一段时间反而不会了，搞得我又重新回来学了一次。

 

 其实KMP算法挺简单的，这个算法的核心我感觉就是在处理next 数组上。

 

 

 我先讲一下一种处理方式吧， next [0]=-1,这个不用多说，第一个肯定是没有匹配好的。

 k=-1; ,i=0两个初始化 ，k,表示的是匹配到的位置 ，i，表示的是你正在为那个位置标记next。

 

 

 
```
 #include<bits/stdc++.h>
using namespace std;
 
void getnext(const char * str,int * next) {
    next[0]=-1;
    int i=0,k=-1;
    while(str[i]!='\0') {
        while(k!=-1&&str[i]!=str[k])k=next[k];//  在你匹配这个 i 之前首先寻找到前面一个最大匹配位置，
        //如果 不匹配就一直往前面回溯，直到能够匹配。
        i++;
        k++;
        if(str[i]==str[k])next[i]=next[k];  //当前位置是相等的 这个不匹配 同样 K的位置也没法匹配，所以 next [i]可以直接等于next[k].
        else next[i]=k;
    }
}

int kmp(const char *s,const char * c)
{
    int lc=strlen(c),ls=strlen(s);
    int *next=new int[lc+1];
    getnext(c,next);
    int j=0,i=0;
    while(i<ls)
    {
       while(j!=-1&&s[i]!=c[j])j=next[j];
        i++,j++;
        if(j==lc)return 1;
    }
    return 0;
} 
 
int main() {
    int next[1000];
    char ch[]="abaabc",s[]="ababaababcb";
    cout<<kmp(s,ch)<<endl;
}

```
 

   
 