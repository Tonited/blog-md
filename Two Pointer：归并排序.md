---
titile: Two Pointer：归并排序
author: 胡凡《算法笔记》
date: 2020.05.17
keywords: Two Pointer：归并排序
img: https://img-blog.csdnimg.cn/20200517150805559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 数据结构与算法
---



归并排序是一种基于“归并”思想的排序方法，本节主要介绍其中最基本的2-路归并排序。

2-路归并排序的原理是，将序列两两分组，将序列归并为 n/2 个组，组内单独排序；然后将这些组再两两归并，生成 n/4 个组，组内再单独排序；以此类推，直到只剩下一个组为止。归并排序的时间复杂度为 O(nlogn)。

## 例子

> 将序列 { 66，12，33，57，64，27，18 }进行2-路归并排序



① 第一趟排序：两两分组，得到四组：{ {66，12}，{33，57}，{64，27}，{18} }。 组内排序得：{ {12，66}，{33，57}，{27，64}，{18} }

② 第二趟排序：将四个组继续两两分组，得到两组 { {12，66，33，57}，{27，64，18} }。组内排序：{ {12，33，57，66}，{18，27，64} }

③ 第三趟排序：将两个组继续两两分组，得到一组 {12，33，57，66，18，27，64}，组内排序：{12，18，27，33，57，64，66}，算法结束。



整个过程如 图4-9 所示

![图4-9](https://img-blog.csdnimg.cn/20200517150805559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)



从上面的过程中可以发现，2-路归并排序的核心在于如何将两个有序序列合并为一个有序序列，而这个过程在[Two Pointer](https://tonited.gitee.io/blog/2020/05/17/two-pointer/)的“序列合并问题”中已经讲解。

接下来讨论2-路归并排序的递归版本和非递归版本的具体实现。

## 递归实现

2-路递归只要把 [left, right] 分成两部分 [left, mid] 和 [mid+!, right]分别递归进行归并排序即可

代码如下，其中 marger 函数为 [Two Pointer](https://tonited.gitee.io/blog/2020/05/17/two-pointer/) 这一章节中代码修改而来

```java
// 将数组 A 的 [L1, R1] 和 [L2, R2]区间合并为有序区间，此处 L2 即为 R1+1
public static void merge(int[] A, int L1, int R1, int L2, int R2){
    int i = 0, j = 0;	// i为A[L1]下标，j为A[L2下标
    int[] temp = new int[(R1 - L1) + (R2 - L2)];	// temp存放临时合并的数数组
    int index = 0;	// temp的下标
    while(i <= R1 && j < R2){
        if(A[i] <= A[j]){
            temp[index++] = A[i++];
        }else{
            temp[index++] = A[j++];
        }
    }
    while(i <= R1)
        temp[index++] = A[i++];
    while(j <= R2)
        temp[index++] = A[j++];
    for(int i = 0; i < index; i++){	// 将temp替换回A的L1到R2
        A[L1 + i] = temp[i];
    }
}
```

```java
// 将array数据当前区间 [left, right] 进行归并排序
public static void mergeSort(int[] A, int[] left, int right){
    if(left < right){
        int mid = left +  (right - left) / 2;	// 防止超限
        mergeSort(A, left, mid);	// 递归左区间 [left, mid]
        mergeSort(A, mid+1, right);	// 递归右区间 [mid+1, right]
        merge(A, left, mid, mid+1, right);	// 将左右子区间合并
    }
}
```





## 非递归实现

2-路归并排序的非递归实现主要考虑到这样一点：每次分组时组内元素个数上限都是2的幂次。

于是就可以想到这样的思路：令步长 step 的初值为 2，然后将数组中每 step 个元素作为一组，将其内部进行排序（即把左 step/2 个元素与右step/2个元素合并，而若元素个数不超过step/2，则不操作）；再令step乘以2

重复上面的操作，直到 step/2 超过元素个数 n

代码如下

```java
public static void mergeSort(int A[]){
    // step 为组内元素个数，step/2 为左子区间元素个数，注意等号可以不写（那就没必要存在①处的判断）
    for(int step = 2; step / 2 <= n; step *= 2){
        // 每 step 个元素一组，组内前 step/2 和后 step/2 个元素进行合并
        for(int i=0; i<n; i+=step){	// 对每一组
            int mid = i + step / 2;	
            if(mid + 1 <= n){	// ①：判断右子区间是否有元素，有就合并
                // 左子区间[i, mid], 右子区间为[mid+1,Math.min(i+step, n)] (这是为了防止超过步长)
                merge(A, i, mid, mid+1, Math.min(i+step, n-1);
            }
        }
    }
}
```

注意点：

1. n 是元素个数，不是最后一个元素的下标，最后元素下标应为 n-1

2. 当最外层循环为 step/2<n 时，就不会出现右子区间无元素的情况，所以①处的判断可以省略

3. 合并时的 右子区间上限 需要判断是否会超过 n-1 （即最后一个元素的下标）

   例如：n 为 5，步长为 4 时

当然，如果时间充足，merge() 处完全可以使用语言API自带的排序