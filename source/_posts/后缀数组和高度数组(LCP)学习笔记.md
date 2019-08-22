---
title: 后缀数组和高度数组(LCP)学习笔记(有坑)
date: 2019-07-30 21:22:27
tags:
    - 后缀数组
    - 笔记
categories:
    - ACM
    - 字符串
mathjax: true
---
## 后缀数组
字符串后缀，指从字符串某个位置开始到字符串末尾的字串，原串和空串也是后缀。反之前缀。
用sa保存字符串开始的下标。
字符串总共有`n+1`个,字符串比较大小是$O(n)$的,所以直接用sort直接排序是$O(n^2log(n))$,很显然不合理。
### 优化一 hash优化
把字符串`hash`处理,修改`sort`排序方式，比较两个字符串，先二分最长前缀，比较两个字符串`hash`处理是$O(1)$的，然后比较第一个位置不同的地方就行了。复杂度$(O(nlog^2(n)))$,但是hash可能会有冲突.
### 优化二 倍增优化
假设一个字符串`abaca`
```
a
ca
aca
baca
abaca
```

后缀如上
一开始比较可以得出,`a<b<c`
得到一个`rank`可以表示为字符大小,然后根据这个排序后缀数组。
```c
a=0
b=1
c=2
//字符串就是
a
a
a
b
c
```
然后开始倍增，比较长度2，
由于已经知道了`a,b,c`，长度为1的大小所以可以直接比较，第一个长度1，的大小，再比较第二个长度为1的大小
最终可以得出`a<ab<ac<ba<ca`
```
a=0
ab=1
ac=2
ba=3
ca=4
//字符串
a
ab
ac
ba
ca
```
详细推断见 **白书P378**
依次类推就行了，详情见代码
这个地方排序可以用**基数排序**，将复杂度优化到$O(nlog(n))$
### 优化三 SA-IS
挖下坑以后填，另外还有`DC3`算法$O(n)$的复杂度，但是`DC3`常数太大.
## 高度数组
这个处理也非常巧妙，我觉得字符串处理都很有意思。
假设一个字符串`abracadabra`
一开始处理出`sa` 数组
你可以发现一个很有意思的事
一开始匹配和`sa[0]` 最大的那一个，也就是原串。
`abracadabra` 后缀排序后比他小的排序后比他小的第一个`sa[7]`就是`abra`
不难看出来，前缀就是`4`个。
`sa[1]` `bracadabra` 匹配 `sa[8]` `bra`
发现了没有，刚好就是 前面的减去一个刚好`3`个。
然后继续匹配
`sa[2]` `racadabra`    `sa[9]` `ra`
刚好`2`个,然后继续
`sa[3]` `acadabra`   `sa[0]` `abracadabra`
刚好`1`个。
所以`sa[i]` 匹配`sa[k]`,虽然下一个`sa[i+1]` 不一定匹配`sa[k+1]` ,但是匹配个数一定至少是$h_i-1$个，然后我们可以直接从$h_i-1$开始匹配就好了。最多增加不会超过`n`次，所以复杂度是$O(n)$的。
详情见 **白书382**

## 推荐习题
[POJ 2217 Secretary](http://poj.org/problem?id=2217)
[POJ 3581 Sequence](http://poj.org/problem?id=3581)
[spoj spoj 694 Distinct Substrings](https://www.spoj.com/problems/DISUBSTR/)

---
## 整理板子
``` c++
namespace My {
    int Rank[maxn + 1], tmp[maxn + 1];
    int k, n;

    bool compare_sa(const int &i, const int &j) {
        if (Rank[i] != Rank[j])return Rank[i] < Rank[j];   //这个地方很巧妙，比较前k
        else {
            int ri = i + k <= n ? Rank[i + k] : -1;   //如果加上后半部分超过n，就直接算最小。
            int rj = j + k <= n ? Rank[j + k] : -1;
            return ri < rj;
        }
    }

    template<class T>
    void construct_sa(T *S, int _n, int *sa) {
        n = _n;
        for (int i = 0; i <= n; i++) {
            sa[i] = i;
            Rank[i] = i < n ? S[i] : -1;
        }
        for (k = 1; k <= n; k *= 2) {
            sort(sa, sa + n + 1, compare_sa);
            tmp[sa[0]] = 0;
            for (int i = 1; i <= n; i++) {
                tmp[sa[i]] = tmp[sa[i - 1]] + compare_sa(sa[i - 1], sa[i]);   //如果两个相等说明前k相等，就像在同一个桶里一样。
            }
            for (int i = 0; i <= n; i++) {
                Rank[i] = tmp[i];
            }
        }
    }

    template<class T>
    void construct_lcp(T *S, int _n, int *sa, int *lcp) {
        n = _n;
        for (int i = 0; i <= n; i++)Rank[sa[i]] = i;
        int h = 0;
        lcp[0] = 0;
        for (int i = 0; i < n; i++) {
            int j = sa[Rank[i] - 1];
            if (h > 0)h--;
            for (; j + h < n && i + h < n; h++) {
                if (S[j + h] != S[i + h])break;
            }
            lcp[Rank[i] - 1] = h;
        }
    }
}

```


