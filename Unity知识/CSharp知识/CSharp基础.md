### 枚举

```c#
//在namespace中声明，在class struct中声明
```

### 数组

```C#
//定义
int arr[];

int arr[] = new int[5];

int arr[] = new int[5] {1, 2, 3, 4, 5};

int arr[] = new int[]  {1, 2, 3, 4, 5, 6, 7, 8};

int arr[] = {1, 2, 3, 4, 5, 6};

arr.Length
// 二维数组
int [ , ] arr;

int [ , ] arr = new int [3, 3];

int [ , ] arr = new int [3, 3]{{1,2,3},{4,5,6},{7,8,9}};

int [ , ] arr = new int {{1,2,3},{4,5,6},{7,8,9}};

int [ , ] arr = {{1,2,3},{4,5,6},{7,8,9}};

arr.GetLength(0);

arr.GetLength(1);
// 交错数组 , 表示数组的数组

int[][] arr;

int[][] arr = new int[3][];

int[][] arr = new int[3][] { new int[] {1, 2, 3}, new int[] {1, 2}, new int {1}};

int[][] arr = new int[][] { new int[] {1, 2, 3}, new int[] {1, 2}, new int {1}};

int[][] arr =  { new int[] {1, 2, 3}, new int[] {1, 2}, new int {1}};
```

### ref 和 out

```c#
// ref和out 可用于引用传值
// ref传入的变量必须初始化，out不用
// out传入的变量必须在内部赋值，ref不用
// 如果一个方法采用 ref 参数，而另一个方法采用 out 参数，则无法重载这两个方法
static void ChangeRef(ref int value);

int a = 1;
ChangeRef(ref a);

static void ChangeOut(out int value);
```

### 变长参数和参数默认值

```c#
// 只能有一个params，而且必须在这组参数的最后
static void Calculate(char step, params int[] arr);

// 可选参数后不能跟普通参数
```
