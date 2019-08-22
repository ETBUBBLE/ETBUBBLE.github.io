---
title: AC自动机
date: 2019-08-07 09:22:59
tags:
    - AC自动机
    - notes
categories:
    - ACM
    - 字符串
mathjax: true
---
## AC自动机用途
给多个字符串`t`，再询问一个字符串`s`，问有多少个字符串`t`出现在询问的字符串`s`中。
## 前置技能
学AC自动机之前，先学会什么是`字典树`，什么是`kmp`。`kmp`我写过一篇博客，就不讲了，就是`next` 数组保存一个最长匹配前缀。字典树就更简单了，每个节点从根节点开始，出现一个字符就在父亲节点上连上下一个节点，也不多说。有需要再写一篇博客。
## AC自动机
对于这个玩意，都说是`字典树`上跑`KMP`,到也没错。朴素`KMP`是在一个串上面跑`next`,而`AC自动机`只是变成了在字典树节点上跑`fail指针`，每个`fail`指针保存是最长匹配后缀。推荐一名[大佬博客](https://blog.csdn.net/bestsort/article/details/82947639)，讲的挺不错的。
朴素的写法，每一个字符串`t`对`s`做一次`kmp`算法或者 对所有的`t`字符串建一颗字典树，然后每个位置匹配一下，很显然$O(n^2)$的复杂度，绝对超时。
仔细思考一下，字典树里面，每次匹配都要从下一个位置开始跑一次匹配，类比一下没有`kmp`的朴素字符串匹配。是不是有点相似。普通单个字符串匹配，是不是枚举每一个位置，然后做一次暴力匹配？然后`kmp` 做了什么，通过`next`找到最大匹配前缀。那么我们是否可以在字典树用`fail`保存最大匹配后缀呢？显然是可以的，不然`AC自动机`干啥.
**举个栗子：** 假设有三个t `asa` `aaa` `aas`, 匹配`aasab`
字典树建成这个样子，丑了点，别在意，重在思想。
![](https://i.loli.net/2019/08/07/SkKvaoYQ6N8ejEV.png)
然后一开始吧`aas`匹配掉了没啥意见吧，然后继续匹配`aasa`，很显然这个时候没有这个字符串，那么肯定就要转移，
转移到哪去呢？最长匹配后缀啊。如图是不是这样。
![](https://i.loli.net/2019/08/07/5pnKxs6ay8C4k23.png)
然后匹配`asa`,再继续匹配`b`,很显然失配了啊，然后找最长匹配后缀，只有根节点了，然后根节点背后又没有b。所以啥都没了，匹配最长后缀为空串。
好了，现在差不多理解这个算法的思想，接下来就是一些细节，不知道我还有没有没有考虑到的，如果有请在评论区提问。
1.上诉所讲的例子很明显，`aas`向`as`跳转，如果后继没有`a`这个节点怎么办?`asa` 改成`asb`,`asa`要匹配谁呢？这个在`kmp`算法里面也有这个问题，很显然，继续找下去就能解决问题，假设 再加一个t`sb`，`aasb`适配，`aas`调转到`as`没有后继`b`,再跳转到匹配后缀`s`，有后继`b`就匹配`sb`，类推，如果还没有找到就继续想上找。**ps: 这个地方可以优化，fail指针找到了最长匹配后缀，然后字典树的下一个节点可以直接跳到对应位置如例子 `aas`后继`a`可以直接跳到为`asa`这个节点，不过不优化也没啥影响，因为字符串长度是固定的，最多不会跳超过`n`次，不优化只是多了个常数**![](https://i.loli.net/2019/08/07/DCchA7SJ1bVlua2.png) 
2.多个匹配怎么办？没到一个节点把他的`fail`找下去记录个数，如图字典树匹配`sa`,的时候会找他的`fail`,找到`a`然后把`a`这个节点记录，同理找`sab`,不仅仅把自己匹配了，还要把`b`匹配掉![](https://i.loli.net/2019/08/07/9L6kgWCQAIio31l.png)
3.怎么记录个数，如果一个字符串多次出现怎么办，看代码，记录一下就行了。

这个AC自动机的思想还是要学好，后面回文自动机要用到这个玩意的思想。

```c
namespace Aho_Corasick_Automaton {
int trie[maxn][27];  //字典树
int cntword[maxn];   //记录单词出现次数，可以开一个vector记录是第几个单词
int fail[maxn];     // 失败回溯
int cnt=0;          // 树的节点个数

void init(int x) {
    for(int i=0; i<26; i++) {
        trie[x][i]=0;
    }
}

void insertWords(char *s) {
    int ls=strlen(s);
    int root=0;
    for(int i=0; i<ls; i++) {
        int next=s[i]-'a';
        if(!trie[root][next]) {
            init(++cnt);
            trie[root][next]=cnt;
        }
        root=trie[root][next];
    }
    cntword[root]++;
}

void getFial() {
    queue<int> q;
    for(int i=0; i<26; i++) {
        if(trie[0][i]) {
            fail[trie[0][i]]=0;
            q.push(trie[0][i]);
        }
    }
    while(q.size()) {
        int now=q.front();
        q.pop();

        for(int i=0; i<26; i++) {
            if(trie[now][i]) {
                fail[trie[now][i]]=trie[fail[now]][i];
                q.push(trie[now][i]);
            } else
                trie[now][i]=trie[fail[now]][i];
        }
    }

}

int query(char *s) {
    int ls=strlen(s);
    int now =0,ans=0;
    for(int i=0; i<ls; i++) {
        now=trie[now][s[i]-'a'];
        for(int j=now; j&&cntword[j]!=-1; j=fail[j]) {
            // 如果这种状态已经计算过了就不用继续找下去了
            ans+=cntword[j];//统计个数，可以在这进行各种操作
            cntword[j]=-1;
        }
    }
    return ans;
}
}

```

---
再贴一个大哥板子
```c
queue<int>q;
struct Aho_Corasick_Automaton {
    int c[N][26],val[N],fail[N],cnt;
    void ins(char *s) {
        int len=strlen(s);
        int now=0;
        for(int i=0; i<len; i++) {
            int v=s[i]-'a';
            if(!c[now][v])
                c[now][v]=++cnt;
            now=c[now][v];
        }
        val[now]++;
    }
    void build() {
        for(int i=0; i<26; i++)
            if(c[0][i])
                fail[c[0][i]]=0,q.push(c[0][i]);
        while(!q.empty()) {
            int u=q.front();
            q.pop();
            for(int i=0; i<26; i++)
                if(c[u][i])
                    fail[c[u][i]]=c[fail[u]][i],q.push(c[u][i]);
                else
                    c[u][i]=c[fail[u]][i];
        }
    }
    int query(char *s) {
        int len=strlen(s);
        int now=0,ans=0;
        for(int i=0; i<len; i++) {
            now=c[now][s[i]-'a'];
            for(int t=now; t&&~val[t]; t=fail[t])
                ans+=val[t],val[t]=-1;
        }
        return ans;
    }
} AC;

```