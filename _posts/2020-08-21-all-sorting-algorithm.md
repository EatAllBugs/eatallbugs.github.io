---
layout: post
title:  排序算法的详解和实例
categories: Algorithm
tags: sorting algorithm
author: Yongsheng
---

* content
[:toc]

所谓排序，将一个数组中的数字按照递增或递减的顺序排列出来。将递增数组reverse可以得到递减数组，所以通常只考虑递增排序。

## 一、排序算法优劣的评价标准

#### 1，稳定性

假定在待排序的记录序列中，存在多个具有相同的关键字的记录，若经过排序，这些记录的相对次序保持不变，即在原序列中，r[i]=r[j]，且r[i]在r[j]之前，而在排序后的序列中，r[i]仍在r[j]之前，则称这种排序算法是稳定的；否则称为不稳定的。

#### 2，时间复杂度

序列由无序到有序消耗的时间度量。

#### 3，空间复杂度

序列由无序到有序消耗的空间度量。

## 二、排序算法的分类详解

### 1，冒泡排序

冒泡排序对两个相邻的元素进行比较，数值小的往前调，数值大的往后调。

#### 基本思想：

首先将第1个和第2个记录的关键字比较大小，如果是逆序的，就将这两个记录进行交换，再对第2个和第3个记录的关键字进行比较，依次类推，重复进行上述计算，直至完成第(n一1)个和第n个记录的关键字之间的比较，此后，再按照上述过程进行第2次、第3次排序，直至整个序列有序为止。排序过程中要特别注意的是，当相邻两个元素大小一致时，这一步操作就不需要交换位置，因此也说明冒泡排序是一种严格的稳定排序算法，它不改变序列中相同元素之间的相对位置关系。

#### 时间复杂度：

最差情况下数组是逆序的，比较次数是1+2+···+(n-1) = (n-1)*n/2。所以时间复杂度是O(n * n) 。

#### 空间复杂度：

没有申请额外的数据结构，所以空间复杂度是O(1)。

#### 稳定性：

当两个元素数值相等时，不需要交换位置，所以是稳定的排序算法。

#### 实例代码：

```c++
class solution{
  public:
    void bubbleSort(int a[]){
        int n = sizeof(a) / sizeof(a[0]);
        int i,j,temp;
        for(int i = 0; i < n; i++){
			for(int j = 0; j < n-i-1; j++){
				if(a[j] > a[j+1]){
                    temp = a[j];
                    a[j] = a[j+1];
                    a[j+1] = temp;
                }
            }
        }
    }
};
```

#### 冒泡算法的改进算法：

上述是单向排序，即大数均往下沉，经过一次比较可以，把最大数的记录送到最后的位置。其实我们可以在使大数往下的同时让小数往上升，这样一次扫描就可以将最大的和最小的记录放到最终位置上，这就是双向冒泡排序思想； 算法改进，双向排序，大数往下，小数往上

```c++
class soution{
	public:
		void bubbleSortDouble(int a[]){
		  	int n = sizeof(a) / sizeof(a[0]);
			int boundmin=0;
			int boundmax=n;
			int min,max,i;
			int temp;
			while(boundmin<boundmax)
			{
				min=0;
				max=0;
				for(i=boundmin;i<boundmax;i++) //大数往下沉
				{
					if(*Array+i)>*(Array+i+1))
					{
						temp= *(Array+j);
						*(Array+j) = *(Array+j+1);
						*(Array+j+1) = temp;
						max=i;	//max记录下沉时候最后一次发生数据交换的位置
					}
				}
				if(max==0)	//本次扫描没有记录交换，扫描结束
					break;
				boundmax=max;
				for(i=boundmax-1;i>boundmin;i--)	//小数往上升
				{
					if(*(Array+i)<*(Array+i-1))
					{
						swap(*(Array+j),*(Array+j),temp);
						min=i;
					}
				}
				if(min==0)
					break;
				boundmin=min;
			}
		}
        
      }
};

```



### 2，选择排序

### 3，插入排序

### 4，希尔排序

### 5，归并排序

### 6，快速排序

### 7，基数排序

### 8，堆排序

### 9，计数排序

### 10，桶排序
