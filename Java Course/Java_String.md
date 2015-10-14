#  <center>Java数组与集合<center>

#一、数组
## 1、一位数组
```
int[] x; //定义一个数组变量
x = new int[10]; //实例化数组，数组名（变量）引用了数组空间的首地址
			       x指向数组首元素的地址。
```

## 2、访问元素，通过索引
```
x[5] = 100;   //&x[0]  + 4 * 5     首元素地址+偏移地址

eg:
double[]  d = new int[200];

eg:
计算返回1000以内能被3或者5整除的数字
public int[] fun(int n) {    //
	int len = 0;
	int[] arr = null;
	for (int i = 0; i <= n, i++) {
		if ( i % 3 == 0 || i % 5 == 0) {
			len ++;
		}
	}
	arr = new int[len];
	len = 0;
	for (int i = 0; i <= n, i++) {
		if ( i % 3 == 0 || i % 5 == 0) {
			arr[len++] = i;
		}
	}
	return arr;
}

eg: 
数组初始化
String[]  names = {"li", "zhang", "wang", "liu"};
String name = names[2];

eg:
数组长度
for(int i = 0; i < arr.length; ++i)
	arr[i] = 100;
	
eg:
	//选择排序每次未排序区域选择最小的值，与顶上元素交换
	public void sortArr(int[] arr) {
		for(int i = 0; i < arr.length - 1; i ++){
			int min = i;
			for(int j = i + 1; i < arr.length; j++) {
				if(arr[j] < arr[min]) {
					min = j;
				}
			}
			if (i != min ) {
				int t = arr[min];
				arr[min] = arr[j];
				arr[j] = t;
			}
		}
	}
```

## 3、二维数组

```
int[][] arr = new int[5][10];
arr.length     // 5

Java 二维数组是由一维数组构成的；
int[][] arr = {arr[0], arr[1], ..., arr[4]};
arr[0].length // 10

eg:
	for(int i = 0; i < arr.length; i ++ ) {
		for(int j = 0; j < arr[i]. length; j ++ ) {
			arr[i][j] = (int)(Math.random() * 100);
		}
	}
```

## 4、交错数组
### 1) 定义
```
int[][] x = new int[4][];

x[0] = new int [10];
x[1] = net int[5];
...
x.length  // 4
x[i].length

```

#二、Java集合
##1、数据存储结构
```

```

## 2、集合框架（接口及其实现类构成）
![](http://images.cnitblog.com/i/532548/201404/262238192165666.jpg)

##3、ArryList动态数组
空间大小自动开辟

Java泛型 <T>

```
ArrayList <String> list = new ArrayList<String>();
指明集合ArrayList存储的数据类型为String

ArrayList<Student> list = new ArrayList<Student>();
```
