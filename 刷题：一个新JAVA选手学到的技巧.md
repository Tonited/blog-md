---
titile: 刷题：一个新JAVA选手学到的技巧
author: Tonited
date: 2020.05.12
keywords: 刷题：一个新JAVA选手学到的技巧
categories: 数据结构与算法
---

最近在拿着《算法笔记》刷PAT的一些题目，准备使用JAVA考一次CSP，发现因为算法题JAVA选手不太多，网上很少有针对JAVA选手的指南，于是把这几天查到的、学到的一些技巧整理出来

第一次刷题，学艺不精，我的文章有任何问题或者大家有其他经验技巧的话，很希望能得到大家的指导

这篇文章会随着我的学习一点一点更新完善，有同为JAVA选手的话欢迎大家一起交流

我也写了一些针对于JAVA实现的、我认为能学到些东西的题目，由于PAT没有放宽对JAVA的标准，所以有很多题目时间超限不能AC

我认为算法主要是练习自己的思维能力，学会方法才最重要，所以我对于那些JAVA超时的题目没有花大把时间去优化它们，只求通过可通过测试点

## JAVA选手刷题须知[^1]

- **先写算法代码！先写算法代码！先写算法代码！**

  不要先处理输入输出和数据转换的代码，不然代码写着写着就会思路混乱

- 首先确保没有加package，类名称为Main。

- 为了运行效率，请使用

  ```
  import java.io.BufferedReader;
  import java.io.InputStreamReader;
  import java.io.IOException;
  BufferedReader bf=new BufferedReader(new InputStreamReader(System.in));
  ```

  - 因为PAT系统对scanner支持不友好且运行时间长。
  
 - PAT是单点测试，就是每个测试点只有一组输入

 - codeup则是多点测试，处理输入需要使用while()

- 请在使用完bufferedreader之后立刻使用close()；方法关闭，否则可能会发生内存泄漏（关闭的越早越好）。

- 【重要】请不要随便import没有用到的包，亲测PAT若是导入了java.util.Scanner可是你没有用到scanner，就会返回非零。


- 一般对于100ms时间限制的题目，基本ac不了，哪怕优化得再好。因为很多乙级题目运行时长（该死的JVM启动）在100ms上下，运气好AC的多，运气差全超时！
- 200ms以上的题目，若是运行超时，那就请不要用暴力破解。
- 还是超时的话，建议换语言。官方说明：选择合适的语言也是一种技巧，所以不给你JAVA放宽时间限制

## 代码技巧
### 一、JAVA使用
#### 1 数学处理
1. **Math.round()四舍五入**
	 功能四舍五入，注意返回值：int（float）、float（double），int/2.0默认double
	
2. **两int相加考虑是否超限，超限使用Long承接**

		如果两个int相加会超过范围，必须使用Long（C的longlong）**
3. **JAVA的int范围是-2^31——2^31-1，即-2147483648——2147483647，比C语言的int范围大的多**
4. **字符串转数字，如果初始是空串，注意最后判断是空串，把空串转化为零**
	```java
	String i = "";
	int s = 10;
	while(s-->0){
		i+="6";
	}
	// 记得处理空串
	if(i.equals("")) i = 0;
	res = Long.parseLong(i) + 10;
	```
5. **任何进制字符串 转 int**
	```java
	int i = Integer.parseInt("E8", 16); //十六字符串转int，16代表前的的是十六进制数值
	```
6. **int 转 16进制字符串**
	```java
	String s = Integer.toHexString(i); //int转十六进制字符串
	```
7. **大数字符串转换为表示科学计数法**
	```java
	import java.math.BigDecimal;
	BigDecimal s =  new BigDecimal(str);
	```
8. **for(char i:charArr)中对i做修改不影响数组中的数据**
9. **将char[]转化成String需要使用String.valueOf**

	不能用`.toString()`，这转换出来的是地址
10. **作为参数传入函数的变量，如果函数中调用了变量的修改自身的方法才会对本身发生变化**
	```java
	public static void main(String[] args) {
		StringBuilder sb = new StringBuilder("Hello World");
		String st = "Hello World"
		method(sb， st);
	}
	public static void method(StringBuilder stringBuilder， String s) {
		// sb发生了变化，因为“函数中调用了变量的修改自身的方法才会对本身发生变化”
		stringBuilder.append("!")
		// st不变，他没有调用能修改自身的方法
		st += "!";
	}
	```
#### 2 IO、字符、字符串处理

1. **字符串相等别用==用`字符串.equals`**
	字符串==比较的是内存地址，equal比较的是值
2. **Scanner的next()是以空格为终止的**
3. **比较器的使用：比较时间字符串**
	```java
	Employee[] emp = new Employee[100];
	Arrays.sort(emp, new EComparator());
	class Employee {
	    String name;
	    String intime;
	}
	class EComparator implements Comparator<Employee> {
	    public int compare(Employee o1, Employee o2) {
	    	return o1.intime.compareTo(o2.intime);
	    }
	}
	```
4. **JAVA有像C语言一样的输出`System.out.printf()`，可用于控制输出格式，用法与C语言的`printf`基本相同**
	```java
	System.out.printf("%02d, 2);
	```
5.  **字符串以“.”分割需要双斜杠：`String.split("\\.")`** 
6. **0比‘0’的ASCII小48**
#### 3 其他使用
1. **数组转化为List：Arrays.asList(数组)，返回值为List类型**

2. **ArrayList转化为数组：ArrayList.toArray(承接数组)**

3. **数组排序Arrays.sort(类的数组)，List排序 Collections.sort(List<类>); 被排序的类类必须实现Comparable接口。默认为升序**
4. **数组相等Arrays.euqals(数组1，数组2)**
5. **int数组默认值0，引用类型数组默认值null**
6. **迭代Map[^2]**

	```java
	  //第一种：普遍使用，二次取值
	  System.out.println("通过Map.keySet遍历key和value：");
	  for (String key : map.keySet()) {
	   System.out.println("key= "+ key + " and value= " + map.get(key));
	  }
	  
	  //第二种
	  System.out.println("通过Map.entrySet使用iterator遍历key和value：");
	  Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
	  while (it.hasNext()) {
	   Map.Entry<String, String> entry = it.next();
	   System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
	  }
	  
	  //第三种：推荐，尤其是容量大时
	  System.out.println("通过Map.entrySet遍历key和value");
	  for (Map.Entry<String, String> entry : map.entrySet()) {
	   System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
	  }
	 
	  //第四种
	  System.out.println("通过Map.values()遍历所有的value，但不能遍历key");
	  for (String v : map.values()) {
	   System.out.println("value= " + v);
	  }
	```



### 二、简洁代码

1. **使用while替换for更简洁**
	
	```java
	int i = 10;
	while(i-- > 0){}
	```
2. **将大数的每一位存入数组**
	```java
	// n是待存储的数 num是第几位
	while(n != 0){
	    ans[num] = n % 10;
	    num++;
	    n /= 10;
	}
	```
3. **输出数组中每个数字，用空格隔开，最后一个数后无空格**
	```java
	public static void dfs(int n){
	    if(n / 10 == 0){
	        System.out.printf("%s", n % 10);
	        return;
	    }
	    dfs(n / 10);
	    System.out.printf(" %s", n % 10);
	}
	```



### 三、经验技巧

1. **将秒转化为`hh:MM:ss`的格式**
	```java
	System.out.println(
	    String.format("%02d",maxWaitTime/3600) + ":" + // 时
	    String.format("%02d",maxWaitTime%3600/60) + ":" +  // 分
	    String.format("%02d",maxWaitTime%100) // 秒
	);
	```

2. **`compareTo`比较时间字符串（hh:MM:ss）**
	```java
	Employee[] emp = new Employee[100];
	Arrays.sort(emp);
	class Employee implements Comparable{
	    String name;
	    String intime;
	    @Override
		public int compareTo(Object o) {
	        Employee otherEmplyee = (Employee)o;
	        // -1：this属性小于other属性
	        // 0：等于
	        // 1：大于
	        return o1.intime.compareTo(o2.intime);
	    }
	}
	```
3. **“1 10 10 1”是回文， “110101”不是回文**
4. **进位公式**
	```java
	// carry为进位 mod为进制
	// 加法公式
	c[i] = (a[i] + b[i] + carry) % mod;
	// 进位公式
	carry = (a[i] + b[i] + carry) / mod;
	```
5. **如果记不住字符`'0'`的ASCII、想要数字字符的ASCII，可以`'0'+数字`**
6. **将两个大数的每一位装进两个int数组，通过数组对两个数进行运算时要将两长度统一，短的那个数组加长，加长这部分为0**
	不同长度A，B进行运算，补零有以下好处
	- 如果需要reverse可以同时reverse，思路清晰
	- 当一个短一个长时，不会忘记输出长的剩下的那部分

7. **大数格式化（三个数一个逗号：100,000,000）**
	```java
	if(sum >= 1000000)
	    System.out.printf("%d,%03d,%03d", sum/1000000, sum%1000000/1000; sum % 1000);
	else if(sum >= 1000)
	    System.out.printf("%d,%03d", sum/1000, sum%1000);
	else System.out.printf("%d",sum);
	```
8. **分数按大小排序后排名，同分同位**
	- 同分同位：即下一个分数不连续：如五人排名1 2 2 4 5）
	- 如果当前分数不等于上一分数，排名=数组下标+1，否则排名与上一人相同，一般将排名放在排名者的类中
9. **按照分数大小排序，分数相同按照姓名字典序排序**
	 ```java
	 public int compareTo(Object o) {
	    Student othStu = (Student)o;
	    if(this.grade < othStu.grade)
	        return -1;
	    else if(this.grade > othStu.grade)
	        return 1;
	    else 
	        return this.name.compareTo(othStu.name);
		}
	```
10. **升序：12345  ABCD……abcde……**
11. **降序排序**
	- Arrays.sort(数组,Collections.reverseOrder()); 
	- 实现`Comparator`接口比较器实现降序：返回`o1属性-o2的属性`的值
		```java
		public int compare(Student o1, Student o2) {
			return o1.grade - o2.grade;
		};
		```
12. **二分法：也可以对 有序的 时间字符串数组 使用二分法**
	```java
	//二分查找算法 
	//特点:查找速度快。要求:数列必须有序
	public static int binarySearch(int[] nums,int key){
	    int start=0;
	    int end=nums.length-1;
	    int mid=-1;
	    while(start<=end){
	        mid=(start+end)/2;
	        if(nums[mid]==key){
	            return mid;
	        }else if(nums[mid]<key){
	            start=mid+1;
	        }else if(nums[mid]>key){
	            end=mid-1;
	        }
	    }
	    return -1;
	}
	```
13. **计算时间间隔：时间一 不断自增一秒（分|时|天……） 到时间二**
	- 这一方法的优点在于，时间自增过程中可以加入其他运算

	- 例如：计算从时间一开始打电话到时间二 的电话费用，由于每个小时的费用标准都不一样，所以可以每自增一分钟就就这一分钟的费用，加到总时间里（PAT甲级 1016 Phone Bills (25分)）
	
	  ```java
	  while(!Arrays.equals(time1, time2)) {
	      // time1前进一分钟;
	      time1.min++;
	      // 总消耗时间+1
	      totalTime++;
	      // 总消耗 = 当前小时一分钟的价钱（toll）
	      spand += toll[time.hour];
	  
	      // 分钟满60 进位1小时
	      if(time1.min == 60) {
	          time.min = 0;
	          time1.hour++;
	      }
	  
	      // 小时满24 进位1天
	      if(time1.hour == 24) {
	          time1.hour = 0;
	          time1.day++;
	      }
	  }
	  ```
	
14. **字符串转数字，如果初始是空串，注意最后判断是空串，把空串转化为零**
	
	```java
	String i = "";
	int s = 10;
	while(s-->0){
		i+="6";
	}
	// 记得处理空串
	if(i.equals("")) i = 0;
	res = Long.parseLong(i) + 10;
	```
15. **二分法取中点时防止 （ left + right ）/ 2 超过 int 的表示范围，可以使用 left + （ right - left ）/ 2**

[^1]:参考：https://blog.csdn.net/youyuge34/article/details/60143721
[^2]:参考：https://blog.csdn.net/kyi_zhu123/article/details/52769469
