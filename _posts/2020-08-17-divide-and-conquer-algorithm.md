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
| 暴力解法 |        O(N^2)        |    O(1)    |
| 分支算法 | *O*(*N**l**o**g**N*) |  O(logN)   |
| 动态规划 |         O(N)         |    O(1)    |

#### 暴力解法：时间超时

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

#### 贪心解法：源自leetcode题解

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

#### 分治解法：

```c++
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

int maxSubArray2(std::vector<int> &nums) {
    assert(!nums.empty());

    return helper(nums, 0, nums.size() - 1);
}

int helper(std::vector<int> &nums, int left, int right) {
    // 分解到一个值时返回该值
    if (left == right) {
        return nums[left];
    }

    // 求中点值
    int mid = left + (right - left) / 2;

    // 中点左边的最大值
    int leftSum = helper(nums, left, mid);
    // 中点右边的最大值
    int rightSum = helper(nums, mid + 1, right);
    // 横跨中点的最大值
    int croSum = crossSum(nums, left, right, mid);

    // 返回以上三种情况中的最大值
    return std::max(std::max(leftSum, rightSum), croSum);
}

int crossSum(std::vector<int> &nums, int left, int right, int mid) {
    // 分解到一个值时返回该值
    if (left == right) {
        return nums[left];
    }

    // 贪心法求左边的最大值
    int leftSubsum = INT_MIN;
    int curSum = 0;
    for (int i = mid; i > left - 1; i--) {
        curSum += nums[i];
        leftSubsum = std::max(leftSubsum, curSum);
    }

    // 贪心法求右边的最大值
    int rightSubsum = INT_MIN;
    curSum = 0;
    for (int i = mid + 1; i < right + 1; i++) {
        curSum += nums[i];
        rightSubsum = std::max(rightSubsum, curSum);
    }

    return leftSubsum + rightSubsum;
}
```

#### 动态规划：

```c++
/*
 * 方法三 动态规划—— Kadane算法 O(n)
 *
 * 在整个数组或在固定大小的滑动窗口中找到总和或最大值或最小值的问题，
 * 可通过动态规划(DP)在线性时间内解决
 *
 * 两种标志DP适用于数组：
 * 1. 常数空间，沿数组移动并子啊原数组修改；
 * 2. 线性空间，首先沿left->right方向移动，然后沿right->left方向移动，最后合并结果。
 *
 * 本题可通过修改数组跟踪当前位置的最大和，
 * 在知道当前位置的最大和后更新全局最大和。
 * */

int maxSubArray3(std::vector<int> &nums) {
    assert(!nums.empty());

    int n = nums.size();
    int maxSum = nums[0];

    // 如果当前值小于0，
    // 重新开始(全局最大值更新)
    for (int i = 1; i < n; i++) {
        // 更新当前的最大值
        if (nums[i - 1] > 0) {
            nums[i] += nums[i - 1];
        }
        // 更新全局的最大值
        maxSum = std::max(nums[i], maxSum);
    }

    return maxSum;
}
```



### 2，二分查找

![](/images/leetcode_53.png)

第一种暴力解法，遍历一遍，时间复杂度是O(n)。可是遍历的解法没有利用到数组是有序的这一条件。采用分治算法，将数组二等分，逐步判断target所在的区域，执行log(n)次就能找到结果。分治算法的时间复杂度是O(log(n))。

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

### 3,大整数乘法

![](D:\github_code\eatallbugs.github.io\images\leetcode-43.png)

通常将大数转化成字符串进行处理。

#### KaraTsuba乘法：



#### 分治算法求解：

```

```

