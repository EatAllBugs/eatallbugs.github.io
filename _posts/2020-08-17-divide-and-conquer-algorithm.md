---
layout: post
title:  分治算法详解和实例解析
categories: Algorithm
tags: Data-structure algorithm
author: Yongsheng
---

* content
{:toc}
分治法的设计思想是：将一个难以直接解决的大问题，分割成一些规模较小的相同问题，以便各个击破，分而治之。



## 一，基本思想和概念

把一个复杂的问题分成两个或更多的相同或相似的子问题，再把子问题分成更小的子问题……直到最后子问题可以简单的直接求解，原问题的解即子问题的解的合并。

## 二，分治算法的步骤

1，将原问题分解为若干个规模较小，相互独立，与原问题形式相同的子问题。

2，若子问题规模较小而容易被解决则直接解，否则递归地解各个子问题。

3，将各个子问题的解合并为原问题的解。

## 三，分治算法的例子

（1）二分搜索

（2）大整数乘法

（3）Strassen矩阵乘法

（4）棋盘覆盖

（5）合并排序

（6）快速排序

（7）线性时间选择

（8）最接近点对问题

（9）循环赛日程表

（10）汉诺塔

（11）最大连续子数组的和

## 四，分治算法的实例

### 1，求最大连续子数组的和

![](/images/leetcode_53.png)

| 常见解法 |      时间复杂度      | 空间复杂度 |
| :------: | :------------------: | :--------: |
| 暴力解法 |        O(N^2)        |            |
| 分支算法 | *O*(*N**l**o**g**N*) |            |
| 动态规划 |         O(N)         |            |

##### 暴力解法：时间超时

```c++
    int maxSubArray(vector<int>& nums) {
        if(nums.size() == 0){return 0;}
        if(nums.size() == 1) {return nums[0];}
        int max = nums[0];
        int sum = 0;
        for(int i = 0 ; i < nums.size();i++){
            sum = nums[i];
            max = max > sum ? max : sum;
            for( int j = i+1 ; j < nums.size();j++){
                sum += nums[j];
                max = max > sum ? max : sum;
            }
        }
        return max;
```

##### 贪心解法：源自leetcode题解

```c++
/*
 * 方法一 贪心法 O(n)
 *
 * 当叠加的和小于0时，就从下一个数重新开始，
 * 同时更新最大和的值(最大值可能为其中某个值)，
 * 当叠加和大于0时，将下一个数值加入和中，
 * 同时更新最大和的值，依此继续。
 *
 * 举例： nums = [-2,1,-3,4,-1,2,1,-5,4]
 * sum = INT_MIN <= 0-> sum = -2 <= 0 -> sum = 1 > 0 ->
 * -> sum = -2 <= 0 -> sum = 4 > 0 -> sum = 3 > 0 ->
 * -> sum = 5 > 0 -> sum = 6 > 0 -> sum = 1 > 0 ->
 * -> sum = 5 > 0
 * res = [-2, 1, 1, 4, 4, 5, 6, 6, 6]
 * 最终返回 res = 6
 * */
int maxSubArray(vector<int>& nums) {
      if(nums.size() == 0){return 0;}
      if(nums.size() == 1) {return nums[0];}
      int max = INT_MIN;
      int sum = 0;
      for(int i = 0 ; i < nums.size();i++){
          if(sum < 0){ 
              sum = nums[i];
          }else{
              sum += nums[i];
          }
          if(sum > max){
              max = sum;
           }       
      }
      return max;
}
```

##### 分治解法：

```
/*
 * 方法二 分治法 O(nlogn)
 *
 * 分治法模板：
 * 1. 定义基本情况
 * 2. 将问题分解为子问题并递归解决子问题
 * 3. 合并子问题的解以获得原始问题的解
 *
 * 将nums由中点mid分为三种情况：
 * 1. 最大子串在左边
 * 2. 最大子串在右边
 * 3. 最大子串跨中点，左右都有
 *
 * 当子串在左边或右边时，继续分中点递归分解到一个数为止，
 * 对于递归后横跨的子串，再分治为左侧和右侧求最大子串，
 * 可使用贪心算法求最大子串值，再合并为原始的最大子串值
 * */

```

### 2，二分查找

![](/images/leetcode_53.png)

代码：

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int low = 0;
        int high = n-1;
        //注意边界条件，左值小于右值
        while(low <= high){
            int mid = (low + high)/2;
            if(target == nums[mid]){
                return mid;
            }else if(target > nums[mid]){
                low = mid + 1;
            }else if(target < nums[mid]){
                high = mid - 1;
            }
        }
        return -1;
    }
};
```





