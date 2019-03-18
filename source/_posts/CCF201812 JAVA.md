---
title: CCF201812 JAVA
date: 2019-01-16 18:41:56
tags: CSDN迁移
---
  发现JAVA比C++慢了不止一星半点

 C++能直接过，这个JAVA卡极限过了

 第一二题，随便写一下没啥难度，直接写第4题的代码

 
# CCF201812-4

 
```
 
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner input=new Scanner(System.in);
        int n,m,r;
        n=input.nextInt();
        m=input.nextInt();
        r=input.nextInt();
        Point arr[]=new Point[m];
        for(int i=0;i<m;i++)
            arr[i]=new Point();
        for(int i=0;i<m;i++){
            arr[i].x=input.nextInt();
            arr[i].y=input.nextInt();
            arr[i].c=input.nextInt();
        }
        Arrays.sort(arr, new MyComprator());
        int ans=0;
        int k=0;
        int []par=new  int[m+1];
        for(int i=1;i<=m;i++)par[i]=i;
        for(int i=0;i<m;i++){
            int x=find(arr[i].x,par),y=find(arr[i].y,par);
            if(x!=y) {
                par[x] = y;
                ans = arr[i].c;
                k++;
            }
            if(k==n-1)break;
        }
        System.out.println(ans);
    }
    static int find(int x,int[] par){
        return x==par[x] ? x : (par[x]=find(par[x],par));
    }
}

class Point{
    int x,y,c;
}


class MyComprator implements Comparator {
    public int compare(Object  arg0, Object  arg1) {
        Point t1=(Point)arg0;
        Point t2=(Point)arg1;
        return t1.c>t2.c? 1:-1;
    }
}
```
 

   
 