#算法学习总结 
### 排序算法

![排序算法](https://github.com/LinLshare/algorithms/blob/master/img/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.png?raw=true)

#### 直接插入排序

思路：

使用直接遍历方法在有序区找到插入位置。

算法实现：

```java
void insertSort(int[] R) {
	for (int i = 1; i < R.length; i++) {
		int tmp = R[i];
		int j = i - 1;
		while (j >= 0 && R[j] > tmp) {
			R[j + 1] = R[j];
			j--;
		}
		R[j + 1] = tmp;
	}
}
```

#### 折半插入排序

思路：

使用折半查找方法在有序区找到插入位置。

算法实现：

```java
void insertSort2(int[] R) {
	for (int i = 0; i < R.length; i++) {
		int tmp = R[i];
		int low = 0;
		int high = i - 1;
		while (low <= high) {
			int mid = (low + high) / 2;
			if (tmp > R[mid]) {
				low = mid + 1;
			} else {
				high = mid - 1;
			}
		}
		for (int j = i - 1; j >= high + 1; j--) {
			R[j + 1] = R[j];
		}
		R[high + 1] = tmp;
	}
}
```

#### 冒泡排序

思路：

从右到左相邻两元素两两比较，小者上浮到有序区。

算法实现：

```java
void bubbleSort(int[] R) {
	boolean isExchange = false;
	for (int i = 0; i < R.length - 1; i++) {
		// 从后面将小的冒起
		for (int j = R.length - 1; j > i; j--) {
			if (R[j] < R[j - 1]) {
				int tmp = R[j];
				R[j] = R[j - 1];
				R[j - 1] = tmp;
				isExchange = true;
			}
		}
		if (!isExchange) {
			return;
		}
	}
}
```

#### 快速排序

思路：

任取一元素将无序区分割成两个，然后两个无序区分别再选取一元素进行分割，直到剩余元素为1。

算法实现：

```java
void quickSort(int[] R, int s, int t) {
	if (s >= t) {
		return;
	}
	// 取一元素为基准，R[s]位置空余
	int tmp = R[s];
	int i = s, j = t;
	while (i != j) {
      //从右到左比较
		while (j > i && R[j] >= tmp) {
			j--;
		}
      //找到一个小于tmp的放到左边
		R[i] = R[j];
      //从左到右比较
		while (i < j && R[i] <= tmp) {
			i++;
		}
      //找到一个大于tmp的放到右边
		R[j] = R[i];
	}
	//插入
	R[i] = tmp;
	quickSort(R, s, i - 1);
	quickSort(R, i + 1, t);
}

void quickSort2(int[] R, int s, int t) {
	if (s >= t) {
		return;
	}
	int q = s;
	for (int u = s; u < t; u++) {
      //将小于或等于最右边的基准元素的放置在最左边，并用q确定边界
		if (R[u] <= R[t]) {
			int tmp = R[u];
			R[u] = R[q];
			R[q] = tmp;
			q++;
		}
	}
  //插入基准元素
	int tmp = R[t];
	R[t] = R[q];
	R[q] = tmp;
	quickSort2(R, s, q - 1);
	quickSort2(R, q + 1, t);
}
```

#### 直接选择排序

思路：

每次从无序区选出一个最小的放置到有序区。

算法实现：

```java
void selectSort(int[] R) {
	for (int i = 0; i < R.length; i++) {
		int k = i;
		for (int j = i + 1; j < R.length; j++) {
			if (R[j] < R[k]) {
				k = j;
			}
		}
		if (k != i) {
			int tmp = R[k];
			R[k] = R[i];
			R[i] = tmp;
		}
	}
}
```

#### 堆排序

思路：

通过一个完全二叉树，每次建堆（二叉堆）使一个元素归位。

二叉堆：完全二叉树，且满足

1）Ki <= K2i 且 Ki <= K2i+1 （小根堆）或

2）Ki >= K2i 且 Ki >= K2i+1 （大根堆）

过程：

数组 ——> 大根堆 ——>置换到无序区尾部，循环

算法实现：

```java
// 筛选算法：目的在于将两子堆与父节点一起构成大堆。
// 当发生子节点与父节点交换时，该子节点原先的堆可能被打破，需要向下迭代建堆。
void sift(int[] R, int low, int high) {
	int i = low, j = 2 * i+1; //R[j]为R[i]左孩
	int  tmp = R[i];
	while (j <= high) { // j = high时只有左孩
		if (j < high && R[j + 1] > R[j]) {
			j++;
		}
		if (R[j] > tmp) {
			// 有子节点大于父节点，继续向下遍历
			R[i] = R[j];
			i = j; // 指向被置换的孩子
			j = 2 * i;
		} else {
			// 没有子节点大于父节点，结束向下遍历
			break;
		}
		R[i] = tmp;
	}
}

// 堆排序算法
void heapSort(int[] R, int n) {
	// 从叶子节点开始建堆
	for (int i = n / 2; i >= 0; i--) {
		sift(R, i, n);
	}
	// 将大根堆的首位与无序区的最后一位互换
	for (int j = n; j >= 1; j--) {
		int tmp = R[j];
		R[j] = R[0];
		R[0] = tmp;
		sift(R, 0, j-1);
	}
}
```

#### 归并排序

思路：

将两个有序的子表合并成一个有序的表。每次从两表中取表头的元素比较，小者归位，当无可比较时剩余元素归位。

算法实现：

```java
// 归并两个表,表1：low~mid,表2：mid+1~high
void merge(int R[], int low, int mid, int high) {
	// i指向表1开始，j指向表2开始，k指向新表开始
	int i = low, j = mid + 1, k = 0;
	int T[] = new int[high - low + 1];

	// 遍历比较
	while (i <= mid && j <= high) {
		if (R[i] <= R[j]) {
			T[k] = R[i];
			i++;
			k++;
		} else {
			T[k] = R[j];
			j++;
			k++;
		}
	}
	// 处理剩余元素
	while (i <= mid) {
		T[k] = R[i];
		i++;
		k++;
	}
	while (j <= high) {
		T[k] = R[j];
		j++;
		k++;
	}
	// 复制元素
	for (k = 0, i = low; i <= high; k++, i++) {
		R[i] = T[k];
	}
}

// 对整个表进行一趟归并
void mergePass(int[] R, int length, int n) {
	int i;
	//表1：i ~ i+length-1
	//表2：i+length ~ i+2*length-1
	//表3：i+2*length ~ i+3*length-1
	for (i = 0; i + 2 * length - 1 < n; i = i + 2 * length) {
		//当下两个相邻表的末尾一个元素大于等于n时跳出循环
		merge(R, i, i + length - 1, i + 2 * length);
	}
	
	if (i + length - 1 < n) {//此时剩下最后的两个相邻表
		merge(R, i, i + length - 1, n - 1);
	}//否则，只剩下最后一张表，不用归并
}


// 从小表合并到大表
void mergeSort(int[] R, int n) {
	int length;
	for (length = 1; length < n; length = 2 * length) {
		mergePass(R, length, n);
	}
}
```

### 查找算法

![查找算法](https://raw.githubusercontent.com/LinLshare/algorithms/master/img/%E6%9F%A5%E6%89%BE%E7%AE%97%E6%B3%95.png)

#### 顺序查找

思路：

按顺序从左到右查找，找不到就递增序号，找到了或者到末尾了就中断查找，最后根据序号判定是否找到。ASL = (n+1)/2.

算法实现：

```java
//顺序查找
int seqSearch(int R[],int n,int key){
	int i=0;
	while(i<n && R[i]!=key){
		i++;
	}
	if(i>=n){
		return -1;
	}else{
		return i;
	}
}

//顺序查找2
int seqSearch2(int R[] ,int n,int key){
	for(int i=0;i<n;i++){
		if(R[i] == key){
			return i;
		}
	}
	return -1;
}
```

#### 折半查找

思路：

每次取中间索引的元素跟key比较，如果等于就返回索引，如果大于key就将high缩小，如果小于key就将low增大。

算法实现：

```java
// 折半查找，假定该表使递增的有序表
int binarySearch(int R[], int n, int key) {
	int low = 0, high = n - 1;
	while (low <= high) {
		int mid = (low + high) / 2;
		if (R[mid] == key) {
			return mid;
		} else if (R[mid] > key) {
			high = mid - 1;
		} else {
			low = mid + 1;
		}
	}
	return -1;
}

```

#### 索引存储结构

使用一个索引表存储元素的关键字和所在地址，根据关键字索引到数据表，然后再找到元素。

索引表的一行：(关键字, 地址) ，按关键字排序。

#### 分块查找

基于索引存储结构的查找算法。特点：

1. 假定分成b块，则
   - 前 n - 1块元素个数 s = ceil( n/b )
   - 最后一块元素个数 <= s
2. "分块有序"：前一块最大关键字 < 后一块最小关键字
3. 平均查找长度
   - 折半查找索引表：ASL = lg( b+1 ) + s/2
   - 顺序查找索引表：ASL = 1/2 * ( b + s ) +1

#### 二叉排序树（BST）

定义：是一个空树或者是满足下面条件的二叉树

- 若左子树非空，则左子树上所有元素值均小于根元素的值；
- 若右子树非空，则右子树上所有元素均大于根元素的值；
- 左右子树本身又个是一颗二叉树。

插入、生成以及查找：递归与根节点比较

#### 平衡二叉树

定义：每个节点的左右子树的高度至多相差1的二叉树。

平衡因子：

- 每个节点都有；
- = 左子树高度 - 右子树高度。

著名的平衡二叉排序树：AVL树。

调整方法：

> LL 型，左孩子的左子树上添加节点导致不平衡，解决方法是**单向右旋平衡**。

![LL型二叉树](https://raw.githubusercontent.com/LinLshare/algorithms/master/img/LL%E5%9E%8B%E4%BA%8C%E5%8F%89%E6%A0%91.png)

> RR型，右孩子的右子树上添加节点导致不平衡，解决方式是**单向左旋平衡**。

![](https://raw.githubusercontent.com/LinLshare/algorithms/master/img/RR%E5%9E%8B%E4%BA%8C%E5%8F%89%E6%A0%91.png)

> LR型，左孩子的右子树上添加节点导致不平衡，解决方式是**先左旋后右旋平衡**。

![](https://raw.githubusercontent.com/LinLshare/algorithms/master/img/LR%E5%9E%8B%E4%BA%8C%E5%8F%89%E6%A0%91.png)

> RL型，右孩子的左子树上添加节点导致不平衡，解决方式是**先右旋后左旋平衡**。

![](https://raw.githubusercontent.com/LinLshare/algorithms/master/img/RL%E5%9E%8B%E4%BA%8C%E5%8F%89%E6%A0%91.png)

从平衡的二叉排序树种删除一个节点：

1. 先从左子树找到最大节点；
2. 替代要删除的节点；
3. 调整，使平衡。

查找效率：

同样的节点数当其达到最大高度时即是平衡二叉树的最坏情况。此时，类似Fibonacci数列。ASL = O( lgn )。