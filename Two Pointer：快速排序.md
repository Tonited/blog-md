---
titile: Two Pointer：快速排序
author: 胡凡《算法笔记》
date: 2020.05.19
keywords: Two Pointer：快速排序
mathjax: true
img: https://img-blog.csdnimg.cn/20200519071452379.png
categories: 数据结构与算法
---



## 首先解决的问题

快速排序平均时间复杂度 O（nlogn)

它的实现要先解决解决了这样的一个问题：

> 对一个序列 A[1]、A[2]、……、A[n]。调整序列中元素的位置，使得 A[1]（原序列的 A[1]，下同）的左侧所有元素都不超过 A[1]、右侧所有元素都大于 A[1]。例如序列 { 5，3，9，6，4，1 } 来说，可以把 A[1]=5 调整到满足条件的位置，如序列 { 3，1，4，5，9，6 }，这样 5 的左侧元素都不超过它，右侧元素都超过它，如图 4-10
>
> ![图4-10](https://img-blog.csdnimg.cn/20200519071303233.png)



这个问题很多方案解决，下面给出速度最快的方案，思路就是 [Two Pointer](https://tonited.gitee.io/blog/2020/05/17/two-pointer/)

1. 先将 A[1] 存储在某个临时变量 temp，并令两个下标 left，right 分别指向序列的首尾（left=1，right=n）

   ![示意图](https://img-blog.csdnimg.cn/20200519071351707.png)

2. 只要 right 指向的元素 A[right]>temp，就将 right 不断左移；直到某时 A[right]<=temp，将元素 A[right] 移动到 left 指向的元素 A[left] 处

   ![示意图](https://img-blog.csdnimg.cn/20200519071412687.png)

3. 只要 left 指向的元素 A[left]<temp，就将 left 不断右移；直到某时 A[left]>=temp，将元素 A[left] 移动到 right 指向的元素 A[right] 处

   ![示意图](https://img-blog.csdnimg.cn/20200519071435887.png)

4. 重复 2、3 两步，直到 left 与 right 相遇，把 temp（也就是原 A[1] ）放到相遇的地方

   ![示意图](https://img-blog.csdnimg.cn/20200519071452379.png)

   ![示意图](https://img-blog.csdnimg.cn/20200519071543332.png)

   ![示意图](https://img-blog.csdnimg.cn/20200519071548357.png)
   
   



很容易写出这部分代码，其中用以划分区间的元素 A[left] 称为主元

```java
// 对区间 [left, right] 进行划分
public static int Partition(int[] A, int left, int right){
    int temp = A[left];	// A[left]存在临时变量temp
    while(left < right){	// 只要 left 和 right 不相遇
        while(left < right && A[right] > A[left])
            right--;	// 反复左移right
        A[left] = A[right];
        while(left < right && A[left] < A[right])
            left++;
        A[right] = A[left];
    }
    A[left] = temp;
    return left;	// 返回相遇点
}
```



## 快排主体代码

接下来实现快速排序算法，它的思路是：

1. 调整序列中的元素，使 当前序列最左端的元素 在调整后满足：左侧所有元素均不超过该元素、右侧所有元素均大于该元素
2. 对该元素的左侧右侧分别进行 步骤1 的调整，直到当前调整区间的长度不超过 1

快速排序的递归实现如下：

```java
// 快速排序，left 和 right 初值为序列首位下标（例如 1 与 n）
public static void quickSort(int[] A, int left, int right){
    if(left < right){	//当前区间长度超过1
        // 将 [left, right] 按照 A[left] 一分为二
        int pos = Partition(A, left, right);
        quickSort(A, left, pos - 1);	// 左区间递归排序
        quickSort(A, pos + 1, right);	// 右区间递归排序
    }
}
```



## 随机-快速排序

### 时间复杂度遇到的问题

快速排序算法当序列中元素的排列比较随机时效率最高，但是当序列汇总元素接近有序时，会达到最坏时间复杂度 O（$n^2$），产生这种情况的主要原因在于主元没有把当前区域划分为两个长度接近在子区间。

有什么办法解决这问题，其中一个办法是随机选择主元

> 也就是对 A[left, right] 来说，不总是用 A[left] 作为主元，而是从这段序列中随机选择一个作为主元
>
> 这样虽然最坏时间复杂度仍是  O（$n^2$ ）（例如，每次随机到的都是 A[left] 作为主元 ），但是对任意输入数据的期望时间复杂度都能达到 O（nlogn）
>
> 也就是说，不存在一组特定的数据能使这个算法出现最坏的情况（详细证明可以参考算法导论）

### 生成随机数

那么如何生成随机数，C语言和Java有生成随机数的代码

如果想生成给定范围 [a, b] 内的随机数，那就是 `Math.random() * (b - a + 1) + a`（Java的Math.random（）生成的是[0, 1]）

> 显然：Math.random（） * ( b - a + 1 ) 的范围是 [0, b-a]，加上 a 就是 [a, b-1]
>
> C语言的 rand() 函数的范围是 [0, RAND_MAX]，不同系统中 RAND_MAX 的值不同，这里取 32767，那生成随机数就可以使用 `rand() % (b - a + 1) + a`或 `(int)((double)rand() / 32767 * (b - a + 1) + a`

### 完整代码

```java
// 交换数组中的两个元素
public static void swap(int[] a, int i, int j) {
    int t = a[i];
    a[i] = a[j];
    a[j] = t;
}

public static int randPartition(int[] A, int left, int right){
    // 生成[left, right]内部的随机数p
    int p = Math.random() * (right - left + 1) + left;
    
    swap(A, left, p);	// 交换 A[p] 和 A[left]
    // 以下为原 randPartition 函数，无需任何改动
    int temp = A[left];
    while(left < right){
        while(left < right && A[right] > temp) {
            right--;
        }
        A[left] = A[right];
        while(left < right && A[left] < temp){
            left++;
        }
        A[right] = A[left];
    }
    A[left] = temp;
    return left;
}

// 快速排序，left 和 right 初值为序列首位下标（例如 1 与 n）
public static void quickSort(int[] A, int left, int right){
    if(left < right){	//当前区间长度超过1
        // 将 [left, right] 按照 A[left] 一分为二
        int pos = Partition(A, left, right);
        quickSort(A, left, pos - 1);	// 左区间递归排序
        quickSort(A, pos + 1, right);	// 右区间递归排序
    }
}
```

