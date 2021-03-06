---
title: Acwing算法基础课学习笔记
date: 2021-02-22 22:52:07
tags:
---

# 2.22 acwing 算法基础课 笔记

第二次听第一课了

主要是觉得没有听进去 重新听一遍。

第一课：两个内容，排序和二分

排序：快排，归并排序

二分：整数二分，浮点数二分



tips:

​	1.背过 模板 -》理解思想，能调试通过 

​	2.题目，重复三到五次





## 快排——分治

1. 确定分界点：q[l], q[(l+r) / 2], q[r] ,随机
2. 调整区间：![image-20210222215538451](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210222215538451.png)
3. 递归处理两边



简单思想：

![image-20210222215804961](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210222215804961.png)

优美的方法：

```C++
#include <iostream>

using namespace std;

const int N  = 1e6 + 10;
int n;
int q[N];

void quick_sort(int q[], int l , int r)
{
    if(l >= r) return ;
    
    int x = q[(l + r) >> 1];
    int i = l - 1;
    int j = r + 1;
    
    
    while(i < j){
        do i ++ ; while(q[i] < x);
        do j -- ; while(q[j] > x);
        if(i < j) swap(q[i], q[j]);
    }
    
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}

int main()
{

    scanf("%d", &n);
    
    for(int i = 0;i < n;++ i ) scanf("%d", &q[i]);
    
    quick_sort(q, 0, n -1);
    
    for(int i = 0;i < n;++ i) printf("%d ", q[i]);
    
    
    return 0;
}

全是细节，我的天
```



# ACWING 算法基础课笔记(二)

## 归并排序

1. 确定分界点： mid = (l + r) >> 1
2. 递归排序 left right
3. 归并——合二为一



核心思想 双路归并 双指针

```c++
#include <iostream>

using namespace std;

const int N = 1e6 + 10;

int n;
int q[N],temp[N];

void merge_sort(int &q[], int l, int r)
{
    if(l >= r) return;
    
    int mid = (l + r) >> 1;
    
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);
    
    int k = 0,i = l, j = mid + 1;
    while(i <= mid && j <= r) {
		if(q[i] <= q[j]) temp[k++] = q[i++];
        else temp[k++] = q[j++];            
    }
    
    while(i <= mid)   temp[k++] = q[i++];
    while(j <= r)  temp[k++] = q[j++];
    
    for(i = l, j = 0;i <= r;i++,j++) q[i] = temp[j];
}

int main()
{
    scanf("%d", &n);
    for(int i = 0;i < n;++i) scanf("%d", &q[i]);
    
    merge_sort(q, 0, n - 1);
    
    for(int i = 0;i < n;++i) printf("%d ", q[i]);
    
    return 0;
}

```

