---
titile: 递归：N皇后问题
author: Tonited
date: 2020.02.09
keywords: 递归：N皇后问题
img: https://img-blog.csdnimg.cn/20200208161411737.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 数据结构与算法
---

了解N皇后问题之前，可以先了解[递归：全排列问题](https://tonited.gitee.io/blog/2020/02/08/di-gui-quan-pai-lie-wen-ti/)

## N皇后问题
在`N*N`的国际象棋盘上放置`N`个皇后，要求每个皇后的：同一行，同一列，对角线不会出现其他皇后，输出合法方案的个数
![示意图](https://img-blog.csdnimg.cn/20200208161411737.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

## 解析
如果采用组合数方式枚举法的话，从n^2^个位置找出n个位置，需要C^n^~nxn~的枚举量，n=8时就是54 502 232次枚举
换个思路，由于每行每列都只放一个皇后，那么把每列标记一个列号`i`，每行标记一个行号`j`，在第`i`列中，皇后出现的行数关系为`line[i] = j`，于是只要[枚举](https://blog.csdn.net/weixin_43553694/article/details/104223331)1~n的所有排列，之后查看每个放置方案是否合法即可

- 两个皇后是否处于同一对角线，只要判断两个皇后的`行号差`与`列号差`的绝对值是否相同就可以啦
	```java
	if( Math.abs(i1 - i2) == Math.abs( line[i1] = line[i2] )
	```



## 代码实现

### 暴力法

```java
// N皇后问题
private static int N;

// 合法方案的个数，初始为0
private static int count = 0;

// hashTable表示 第i列 上是否已经有皇后了
// 0位空着不用，这样更方便理解
private static boolean[] hashTable = new boolean[N+1];

// 存放当前皇后的排列方案，表示第i列中的皇后放在了第Column[i]行
// 其实就是全排列的一个方案
// 从i=1开始，0位空着不用，这样更方便理解
private static int[] Column = new int[N+1];

// index表示当前处理的是第几列
public static void generateColumn(int index) {
	// 如果当前位数是第n列（index是从1开始的），说明已经排完了
	if(index == N+1) { 
		// 表示当前排列方式是否有效
		boolean isUseful = true;
		
		// 判断当前方式是否有效
		// 方法：遍历任意两个皇后，判断是否有两个皇后在同一对角线上
		for(int i=1; i<=N; i++) {
			for(int j=1; j<=N; j++) {
				// 如果有两个皇后在同一对角线上，本方式就无效
				if(Math.abs(i - j) == Math.abs(Column[i] - Column[j])) {
					isUseful = false; // 设为无效
				}
			}
		}
		
		// 有效就输出整个方案（排列）
		if(isUseful == true) {
			System.out.print("排列：");
			for(int i=1; i<N; i++) {
				System.out.print(Column[i]);
			}
			System.out.println("有效");
			// 有效方案+1
			count++;
		}
		return;
	}
	
	// 对于第index列（当前列）的每一个行，都尝试放皇后
	for(int i=1; i<=N; i++) {
		if(hashTable[i] == false) { // 第i行在前几列中没有出现过皇后
			//皇后放到第index列的第i行上
			Column[index] = i; 
			// 把第i行标记成已经有皇后了
			hashTable[i] = true; 
			// 处理子问题（下一位往后
			generateColumn(index + 1); 
			// 处理完子问题，把i标记还原，以保证当前位位可以使用
			hashTable[i] = false; 
		}
	}
}
```





枚举所有情况，然后判断每一种情况是否合法的做法时非常朴素的，因此一般把不用优化算法，直接朴素算法解决的做法叫暴力法



### 回溯法 

事实上，通过思考可以发现，如果某一种方案在第`i`列放置皇后时，已经出现了不合法的情况（有两个皇后对角线），剩下的列和皇后其实就没必要放了，所以我们没放一个皇后都判断一下是否合法，如果不合法，就不必处理当前皇后及其子问题了

一般来说，如果达到递归边界前，由于某些原因已经不需继续递归，就可以直接返回上层，这种做法叫回溯法



```java
// index表示当前处理的是第几列
public static void generateColumn(int index) {
	// 如果当前位数是第n列（index是从1开始的），说明已经排完了
	// 能到这里一定是合法的
	if(index == N+1) { 
		System.out.print("排列：");
		for(int i=1; i<N; i++) {
			System.out.print(Column[i]);
		}
		System.out.println("有效");
		// 有效方案+1
		count++;
		return;
	}
	
	// 对于第index列（当前列）的每一个行，都尝试放皇后
	for(int i=1; i<=N; i++) {
		if(hashTable[i] == false) { // 第i行在前几列中没有出现过皇后

			// 当前皇后如果放到当前位置（第index列第i行），与之前的皇后是否会有冲突（对角线）
			boolean conflict = false;
			
			// 判断当前皇后如果放到当前位置（第index列第i行），与之前的皇后是否会有冲突（对角线）
			for(int pre = 1; pre < index; pre++) {
				// 当前位置的皇后与之前位置的皇后是否成对角线
				// 当前位置行号为i, pre列皇后行号为Column[pre]
				if(Math.abs(index - pre) == Math.abs(i - Column[pre])) {
					conflict = true;
					break;
				}
			}
			
			// 如果没有冲突执行
			// 有冲突不解决子问题，直接处理同级下一问题
			if(conflict == false) {
				//皇后放到第index列的第i行上
				Column[index] = i; 
				// 把第i行标记成已经有皇后了
				hashTable[i] = true; 
				// 处理子问题（下一位往后
				generateColumn(index + 1); 
				// 处理完子问题，把i标记还原，以保证当前位位可以使用
				hashTable[i] = false; 
			}
		}
	}
}
```
