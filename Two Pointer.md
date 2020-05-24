---
titile: Two Pointer
author: 胡凡《算法笔记》
date: 2020.05.17
keywords: Two Pointer
mathjax: true
img: https://img-blog.csdnimg.cn/20200517134635702.png
categories: 数据结构与算法
---

很少有教材拿出来讲，因为Tow Pointer不像是一种算法，更像是一种编程思想

广义上的 Two Pointer 是利用问题本身与序列的特性，使用下标 i, j 对序列进行扫描，以较低复杂度（一般是O(n)）解决问题



给出以下两个例子



## 找出递增序列中满足a+b=M的a和b的位置

### 问题

> 给定一递增正整数序列和一正整数M，求序列中两个位置的数：a 和 b，满足：a+b=M
>
> 比如给定序列 { 1，2，3，4，5，6 }，M = 8，就存在 2+6=8、3+5=8



### 一般解法

直观解法是二重循环

```java
for(int i = 0; i < n; i++){
    for(int j =i + 1; j < n; j++){
        if(a[i] + a[j] == M){
            System.out.println(i+" "+j);
        }
    }
}
```

时间复杂度 O(n^2^)，对 n 在 10^5^ 的规模时是不可承受的

高复杂度的原因：

- 因为是递增序列，如果出现一对 i 和 j 使 a[i]+a[j]=M，那么显然 a[i]+a[j]>M，a[j]后面的所有数都不用加
- 同理，当出现满足条件的 i 和 j，a[i] 后面的所有数也不用加

能看出 i 和 j 是彼此牵制的，这给优化算法带来很大空间



### Two Pointer解法

令下标 i 的初值为0，下标 j 的初值为 n-1，即令i、j分别指向序列的第一个元素和最后一个元素

接下来根据 a[i]+a[j] 与M的大小来进行下面三种选择，使 i 不断向右移动、使 j 不断向左移动，直到 i>=j 成立，如图4-8所示。

![图4-8](https://img-blog.csdnimg.cn/20200517134635702.png)

- 如果 a[i]+a[j]==M，说明找到了方案，输出。然后由于序列是递增的，所以 a[i+1]+a[j]>M 和 a[i]+a[j+1]>M 都成立，但是a[i+1]和a[j-1]的大小关系未知，因此剩下的方案一定在 [i+1，j-1]中，那么就令 i=i+1，j=j-1
- 如果 a[i]+a[j]>M，那么a[i+1]+a[j]>M 成立，a[i]+a[j-1] 与 M 的大小关系未知，因此剩下的方案一定在 [i，j-1]中，那么就令 j=j-1
- 如果 a[i]+a[j]<M，那么a[i]+a[j-1]<M 成立，a[i+1]+a[j] 与 M 的大小关系未知，因此剩下的方案一定在 [i+1，j]中，那么就令 i=i+1

```java
while(i <= j){
    if(a[i] + a[j] == M){
        System.out.println(i+" "+j);
        i++;
        j--;
    }else if(a[i] + a[j] > M){
        j--;
    }else{
        i++;
    }
}
```

算法复杂度 O(n)

## 序列合并问题

> 给定两个递增序列 A 和 B，要求把他们合并成一个递增序列 C

Two Pointer方法：设置两个下标 i 和 j，初值都为 0，分别表示指向 A 和 B 的第一个元素

- 如果 A[i]<B[j]，A[i] 加入序列 C，之后 i++
- 如果 A[i]>B[j]，B[j] 加入序列 C，之后 j++
- 如果 A[i]==B[j]，随便哪一个放入 C，对应下标++
- 当 i 或 j 到了对应序列的末尾就停止，然后将另一个序列剩余的元素依次输出



```java
public static int merge(int[] A, int[] B, int[] C, int n, int m){
    int i = 0, j = 0, index = 0;	// i指向A, j指向B, index指向C
    while(i < n && j < m){
        if(A[i] <= B[j]){
            C[index++] = A[i++];
        }else{
            C[index++] = B[j++];
        }
    }
    while(i < A.length)
        C[index++] = A[i++];
    while(j < A.length)
        C[index++] = B[j++];
    return index;	// 返回C的长度
}
```

