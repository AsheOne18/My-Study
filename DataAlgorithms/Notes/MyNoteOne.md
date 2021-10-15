## 1、稀疏数组

### 基本功能

当一个数组中大部分元素为０，或者为同一个值的数组时，可以使用**稀疏数组**来保存该数组。

### 处理方法

- 记录数组**一共有几行几列**，有多少个**不同的值**
- 把具有不同值的元素的行列及值记录在一个小规模的数组中，从而**缩小程序**的规模

![image](https://github.com/AsheOne18/My-Study/blob/main/DataAlgorithms/images/1.png)

如图，把一个6X7的二维数组变为了一个9X3的稀疏数组。其中

- 第一行保存的是原二维数组的行、列以及非0值的个数
- 第二到九行保存的是每个非0值所在的位置及其数值

### 转换思路

二维数组转稀疏数组

- 遍历二维数组，得到二维数组中有效值的个数sum
- 创建稀疏数组，有sum+1行，3列（固定）
- 将二维数组中的有效值存入稀疏数组中

稀疏数组转二维数组

- 先读取稀疏数组的第一行（保存二维数组的行列信息），还原二维数组

- 读取稀疏数组的其他行，将值赋给二维数组的对应位置上的数

  

  **代码**

```java
public class Demo2 {
   public static void main(String[] args) {
      //创建一个二维数组
      int[][] arr1 = new int[11][11];
      //向二维数组里放值
      arr1[1][2] = 1;
      arr1[2][3] = 2;
      arr1[3][4] = 3;

      //打印二维数组
      System.out.println("遍历二维数组");
      for (int i = 0; i < arr1.length; i++) {
         for (int j = 0; j < arr1[0].length; j++) {
            System.out.print(arr1[i][j] + "   ");
         }
         System.out.println();
      }

      //二位数组----->稀疏数组
      //遍历二维数组中有效值的个数,用sum来记录
      int sum = 0;
      for (int i = 0; i < arr1.length; i++) {
         for (int j = 0; j < arr1[0].length; j++) {
            if (arr1[i][j] != 0) {
               //二维数组中元素不为0即为有效值
               sum++;
            }
         }
      }

      //创建稀疏数组
      //行数为sum+1，第一行用于保存二维数组的行列及有效值个数，列数固定为3
      int[][] sparseArr = new int[sum + 1][3];
      //存入二维数组的行列及有效值个数
      sparseArr[0][0] = arr1.length;
      sparseArr[0][1] = arr1[0].length;
      sparseArr[0][2] = sum;

      //再次遍历二维数组，将有效值存入稀疏数组
      //用于保存稀疏数组的行数
      int count = 1;
      for (int i = 0; i < arr1.length; i++) {
         for (int j = 0; j < arr1[0].length; j++) {
            if (arr1[i][j] != 0) {
               //将值存入稀疏数组
               sparseArr[count][0] = i;
               sparseArr[count][1] = j;
               sparseArr[count][2] = arr1[i][j];
               count++;
            }
         }
      }

      //打印稀疏数组
      System.out.println("遍历稀疏数组");
      for (int i = 0; i < sparseArr.length; i++) {
         for (int j = 0; j < sparseArr[0].length; j++) {
            System.out.print(sparseArr[i][j] + "   ");
         }
         System.out.println();
      }


      //稀疏数组------>二维数组
      //先得到二位数组的行列数
      int row = sparseArr[0][0];
      int col = sparseArr[0][1];
      int[][] arr2 = new int[row][col];

      //遍历稀疏数组，同时给二维数组赋值
      for (int i = 1; i < sparseArr.length; i++) {
         row = sparseArr[i][0];
         col = sparseArr[i][1];
         //该位置上对应的值
         int val = sparseArr[i][2];
         arr2[row][col] = val;
      }

      //打印二维数组
      System.out.println("遍历还原后的二维数组");
      for (int i = 0; i < arr2.length; i++) {
         for (int j = 0; j < arr2[0].length; j++) {
            System.out.print(arr2[i][j] + "   ");
         }
         System.out.println();
      }
   }
}Copy
```

运行结果

```java
遍历二维数组
0   0   0   0   0   0   0   0   0   0   0   
0   0   1   0   0   0   0   0   0   0   0   
0   0   0   2   0   0   0   0   0   0   0   
0   0   0   0   3   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
遍历稀疏数组
11   11   3   
1   2   1   
2   3   2   
3   4   3   
遍历还原后的二维数组
0   0   0   0   0   0   0   0   0   0   0   
0   0   1   0   0   0   0   0   0   0   0   
0   0   0   2   0   0   0   0   0   0   0   
0   0   0   0   3   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0   
0   0   0   0   0   0   0   0   0   0   0
```

## 2、队列

### 定义

- 队列是一个**有序列表**，可以用**数组**或是**链表**来实现。
- 遵循**先入先出**的原则。即：先存入队列的数据，要先取出。后存入的要后取出

### 模拟思路

- 队列本身是有序列表，若使用数组的结构来存储队列的数据，则队列数组的声明如下图, 其中 maxSize 是该队列的最大容量
- 因为队列的输出、输入是分别从前后端来处理，因此需要两个变量 front及 rear分别记录队列前后端的下标，front 会随着数据输出而改变，而 rear则是随着数据输入而改变，如图所示

![image](https://github.com/AsheOne18/My-Study/blob/main/DataAlgorithms/images/2.png)

#### 入队出队操作模拟

当我们将数据存入队列时称为”addQueue”，addQueue 的处理需要有两个步骤：

- 将尾指针往后移：rear+1 , 当 **front == rear** 时，队列为空
- 若尾指针 rear 小于队列的最大下标 maxSize-1，则将数据存入 rear所指的数组元素中，否则无法存入数据。**rear == maxSize - 1**时，队列满

**注意**：front指向的是队列首元素的**前一个位置**

**实现代码**

```java
public class Demo3 {
   public static void main(String[] args) {
      ArrayQueue queue = new ArrayQueue(5);
      queue.addNum(1);
      queue.addNum(2);
      queue.addNum(3);
      queue.addNum(4);
      queue.addNum(5);
      System.out.println(queue.getNum());
      queue.showQueue();
   }
}

class ArrayQueue {
   //队列的大小
   int maxSize;
   //用数组来实现队列
   int[] arr;
   //指向队列首元素的前一个位置
   int front;
   //指向队列的尾元素
   int rear;

   public ArrayQueue(int maxSize) {
      this.maxSize = maxSize;
      arr = new int[this.maxSize];
      //front指向队列首元素的前一个位置
      front = -1;
      rear = -1;
   }


   public boolean isFull() {
      return rear == maxSize - 1;
   }


   public boolean isEmpty() {
      return front == rear;
   }


   public void addNum(int num) {
      if(isFull()) {
         System.out.println("队列已满，无法在进行入队操作");
         return;
      }
      //队尾标记后移，指向要放入的元素的位置
      rear++;
      arr[rear] = num;
   }

   public int getNum() {
      if(isEmpty()) {
         throw new RuntimeException("队列为空，无法出队");
      }
      //队首标记后移，指向队首元素
      System.out.print("出队元素是：");
      front++;
      return arr[front];
   }

   public void showQueue() {
      if(isEmpty()) {
         throw new RuntimeException("队列为空，无法遍历");
      }
      System.out.println("遍历队列");
      //从front+1开始读取元素
      for(int start = front+1; start<=rear; start++) {
         System.out.println(arr[start]);
      }
   }
}Copy
```

运行结果

```
出队元素是：1
遍历队列
2
3
4
5Copy
```

### 环形队列

思路：

- front变量指向**队首元素**，初值为0
- rear变量指向队尾元素的**下一个元素**，初值为0。规定空出一个位置
- 队列为空的判定条件：front == rear
- 队列为满的判定条件：(rear + 1) % maxSize == front
- 队列中有效元素的个数：(rear - front + maxSize) % maxSize
- 入队和出队时，都需要让标记**对maxSize取模**

**代码**

```java
public class Demo4 {
   public static void main(String[] args) {
      ArrayAroundQueue aroundQueue = new ArrayAroundQueue(5);
      aroundQueue.addNum(1);
      aroundQueue.addNum(2);
      aroundQueue.addNum(3);
      aroundQueue.addNum(4);
      aroundQueue.showQueue();
      System.out.println(aroundQueue.getNum());
      System.out.println(aroundQueue.getNum());
      aroundQueue.addNum(5);
      aroundQueue.addNum(6);
      aroundQueue.showQueue();
      aroundQueue.getHead();
   }
}

class ArrayAroundQueue {
   //队列的大小
   int maxSize;
   //用数组来实现队列
   int[] arr;
   //指向队列首元素的前一个位置
   int front;
   //指向队列的尾元素
   int rear;

   public ArrayAroundQueue(int maxSize) {
      this.maxSize = maxSize;
      arr = new int[this.maxSize];
      //front指向队列首元素的前一个位置
      front = 0;
      rear = 0;
   }


   public boolean isFull() {
      return (rear+1)%maxSize == front;
   }


   public boolean isEmpty() {
      return front == rear;
   }


   public void addNum(int num) {
      if(isFull()) {
         System.out.println("队列已满，无法在进行入队操作");
         return;
      }
      //先放入元素，在后移队尾标记
      arr[rear] = num;
      rear = (rear+1)%maxSize;
   }

   public int getNum() {
      if(isEmpty()) {
         throw new RuntimeException("队列为空，无法出队");
      }
      //队首标记后移，指向队首元素
      System.out.print("出队元素是：");
      int num = arr[front];
      front = (front+1)%maxSize;
      return num;
   }

   public void showQueue() {
      if(isEmpty()) {
         throw new RuntimeException("队列为空，无法遍历");
      }
      System.out.println("遍历队列");
      //当front + 1 == rear时停止遍历
      int start = front;
      while(start != rear) {
         System.out.println(arr[start]);
         //移动到下一个元素
         start = (start+1)%maxSize;
      }
   }

   public void getHead() {
      if(isEmpty()) {
         throw new RuntimeException("队列为空");
      }

      System.out.println("队首元素为："+arr[front]);
   }
}Copy
```

运行结果

```java
遍历队列
1
2
3
4
出队元素是：1
出队元素是：2
遍历队列
3
4
5
6
队首元素为：3
```
