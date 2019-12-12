---
title: leetcode-4. 寻找两个有序数组的中位数
date: 2019-12-12 08:44:27
tags:
    - 二分
    - leetcode
categories:
    - ACM
    - 二分
mathjax: true
---
这题挺有意思的，一开始想了半天怎么处理细节，想了半天，没想出来，要不暴力所有情况，结果特么还跑的飞快。

这题看一眼肯定知道是二分，两个有序数组，找合并之后的中位数。难搞的就是会出现相同数字，和数组长度是偶数。所以我就搞了一个函数 直接找第 k 个在哪，然后第K个在哪，因为 第 k个可能在第一个数组，又有可能在第二个数组，所以要找两次，找的时候把引用换一下就行了。
贼麻烦，细节太难处理了，细节处理我打在代码注释里。

思想就是 在一个数组里面二分中间值位置，通过在另一个数组里面二分找小于等于当前值的数量和自己的下标相加可以得到在两个数组合并之后的位置。
```c
const int INF=0x3f3f3f3f;
class Solution {
public:
    inline int get(int k,vector<int>& nums1, vector<int>& nums2){   
        int l=0,r=nums1.size()-1;
        while(l<=r){
            int mid=(l+r)/2;
            int num=lower_bound(nums2.begin(),nums2.end(),nums1[mid])-nums2.begin();  //找第二个数组里面小于等于num[mid]的个数
            if(mid+num<=k){  //如果加起来的位置小于  k 说明这个数可能是要找的   因为可能出现很多个相同的数字所以不能简单的判等于
                int t1=upper_bound(nums2.begin(),nums2.end(),nums1[mid])-nums2.begin();  //当前这个值加上相同的个数
                int t2=upper_bound(nums1.begin(),nums1.end(),nums1[mid])-nums1.begin(); //当前这个值加上相同的在num2里面的个数
                if(t1+t2-1>=k)return nums1[mid];  //两个数相加 -1  就是 位置，下标从 0 开始
                /* 举个例子
                 *  {1,1,3,3}  {1,1,3,3}
                 *  假设你要找 k=6
                 *  mid = 2   num1[mid]=3  num=2  mid+num=4  <6
                 *  然后   t1 =  4   t2 = 4   t1+t2-1=7  7>=6  所以 4-7 之间都是相同的数
                 *  所以实际上 3 是找到的答案
                 *  
                 * */
                
                l=mid+1;
            }
            else r=mid-1;
        }
        return -INF;
    }

    inline int Max(int k,vector<int>& nums1, vector<int>& nums2){
        // 因为我如果没有在num1 里面找到 下标刚好为 k 的值就会返回-INF，所以直接取个MAX 就是答案
        // 交换一下引用就相当于从 num2 里面找排名第k的值了
        return max(get(k,nums1,nums2),get(k,nums2,nums1));  
    }

    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int k=(nums1.size()+nums2.size())/2;
        if((nums1.size()+nums2.size())%2==0){  //如果偶数个 就找 两个值
            int t1=Max(k,nums1,nums2),t2=Max(k-1,nums1,nums2);
            return (Max(k,nums1,nums2)+Max(k-1,nums1,nums2))/2.0;
        }
        else return Max(k,nums1,nums2);
    }
};
```