<font style="color:#0099FF;font-size:100px">Data-Struct</font>

- C语言的数组相当于Python的什么?



# 2. 线性表
## 逻辑结构

1. 定义：线性表是相同数据类型的数据元素的有线序列。
2. 特点
- 相同特性：方便用统一的方法处理
- 有限：表中元素的个数是N
- 序列：表中元素排成一列，表中元素除第一个元素外有且仅有一个前驱，除最后一个元素外有且仅有一个后继。


## 存储结构
### 顺序存储


静态分配的顺序表类型定义
```c
# define MaxSize 50  //表的最大长度
typedef struct{
	ElemType data[MaxSize];   //数组元素
	int length;   //当前长度
}SqList 
```

动态分配的顺序表类型定义
```c
# define InitSize 100  //表长初始定
typedef struct{
	ElemType *data;  // 数组指针
	int MaxSize,length; //数组的最大容量和当前个数
}SqList 
```

初始动态分配语句
```c
L.data = (ElemType *)malloc(InitSize*sizeof(ElemType));`
// 用malloc分配一段这么  InitSize*sizeof(ElemType)多个字节的内存段，
// 它返回一个指向该内存段首字节的指针变量，然后把这个指针变量强制转换为ElemType*类型，
// 再把这个经转换的指针变量赋给L的elem成员
```



插入操作：在顺序表第i个位置插入新元素e
```c
bool ListInsert(SqList &L,int i ,ElemType e){
	if (i < 1 || i > L.length+1)       //1.如果插入位置不合理，抛出异常
		return false;
	if (L.length >= MaxSize)           //2.如果线性表长度大于等于数组长度，则抛出异常或动态增加容量
		return false;
	for (int j = L.length;j >= i; j--) //3.从最后一个元素开始向前遍历到第 i 个位置，分别将它们都向后移动一个位置
		L.data[j]=L.data[j-1]
	L.data[i-1] = e; //4.将要插入元素填入位置 i 处
	L.length++; //5.表长加 1
	return ture;
}
```
插入操作平均移动次数：1/(n+1)*n(n+1)/2=n/2

删除操作：删除顺序表L中第i个位置的元素
```c
bool ListDelete(SqList &L,int i ,ElemType &e){
	if (i < 1 || i > L.length+1) //1.如果删除位置不合理，抛出异常
		return false;
	e = L.data[i-1];  //2.取出删除元素
	for (int j = i ; j < L.length; j++) //3.从删除元素开始遍历到最后一个元素位置，分别将它们都向前移动一个位置
		L.data[j-1]=L.data[j]
	L.length--; //4.表长减 1
	return ture;
}
```
插入操作平均移动次数：1/n*n(n-1)/2=(n-1)/2

查找操作：在顺序表L中查找第一个元素值等于e的元素，并返回其位序
```c
int LocateElem(SqList L,int i ,ElemType e){
	int i ;
	for (int i = 0 ; i < L.length; i++) //从第一个元素开始遍历到最后一个元素，如果某元素等于e，则返回该元素下标+1
		if (L.data[i] == e)
			return i+1;
	return 0;
}
```
查找操作平均移动次数：1/n*n(n+1)/2=(n+1)/2

- 顺序表核心特点：
	- 随机存取，所以能在O(1)的时间复杂度内访问任意元素
	- 存储密度高，每个结点只存数据元素
	- 逻辑上相邻的元素物理上也相邻，所以插入和删除需要移动大量元素
    
    
    
### 链式存储
散落在内存中的空间被指针连接了起来
```c
typedef struct LNode
{
    int data;
    struct LNode *next;
}LNode;

LNode *L;
L = (LNode*)malloc(sizeof(LNode));
A->next = B;
B->next = c;
c->next = D
```
### 单链表

含有头结点的链表（常用）：Head指针指向头结点,头结点不存数据

> Head->next == NULL 为真时，链表为空

不含有头结点的链表：Head指针指向开始节点

> Head == NULL 为真时，链表为空

### 双链表

```c
typedef struct DLNode
{
    int data;
    struct DLNode *next;
    struct DLNode *prior;
}LNode;

LNode *L;
L = (LNode*)malloc(sizeof(LNode));
A->next = B;
B->next = c;
c->next = D
```
含有头结点的链表（常用）：Head指针指向头结点（第一个节点不存数据）

> Head->next == NULL 为真时，链表为空

不含有头结点的链表：Head指针指向开始节点（第一个节点存数据）

> Head == NULL 为真时，链表为空


### 循环链表

含有头结点的链表：Head指针指向头结点（第一个节点不存数据）

单循环链表：将最后一个空指针指向头指针：Head->next == head

双循环链表：将最后一个空指针指向头指针：Head->next == head 或 
Head-> prior == head

> Head->next == head  为真时，链表为空

不含有头结点的单双循环链表：Head指针指向开始节点（第一个节点存数据）
Head == NULL 为真

```c 
void createLinkListR(LNode *&head)#
{
	
}
```

头指针与头结点的异同
- 头指针
	- 头指针是指链表指向第一个结点的指针，若链表有头结点，则是指向头结点的指针
	- 头指针具有标识作用，所以常用指针冠以链表的名字
	- 无论链表是否为空，头指针均不为空，**头指针是链表的必要元素**。

- 头结点
	- 头结点是为了操作的统一和方便而设立的，放在第一元素的结点之前，其数据域一般无意义（也可存放链表的长度）
	- 有了头结点，对在第一元素结点前插入结点和删除第一结点，其操作与其它结点的操作就统一了
	- **头结点不一定是链表必要元素**。
    
    
 ---
 ---
# 3. 栈和队列
## 栈
### 栈的基本概念
1. 栈的定义


- 栈（Stack）：只能在一段插入或删除的线性表
- 栈顶（Top）：允许插入或删除的那一端
- 栈底（Button）：固定不允许插入或删除的那一端
- 空栈：不含任何元素的空表


2. 基本操作


- `InitStack(&S)`：初始化
- `StackEmpty(S)`:判空
- `Push(&S,x)`：近栈
- `Pop(&S,x)`：出栈
- `GetTop(S,&x)`：读取栈顶元素
- `ClearStack(&S)`:销毁栈


### 栈的顺序存储结构
1. 顺序栈的实现

利用一组地址连续的存储单元存放自栈底到栈顶的数据元素，同时附设一个指针（Top）指示当前栈顶的位置。

```c
define MaxSize 50 
typedef struct{
	Elemtype data[MaxSize];
	int top;
}SqStack;
```
- 栈顶指针：`S.top`,初始时候`S.top=-1`
- 栈顶元素：`S.data[S.top]`
- 出栈操作：栈非空时，先取栈顶元素值，再将栈顶指针减1
- 栈空条件：`S.top == -1`
- 栈满条件：`S.top == MaxSize-1`
- 栈长：`S.top+1`

2. 顺序栈的基本运算

(1)初始化
```
void InitStack(&S){
	s.top=-1}//初始化栈顶指针
```

(2)判栈空
```
bool StackEmpty(S){
	if(s.top == -1)       //栈空
		return true ;
	else                  //不空
		return false;
	}
```

(3)进栈
```c
bool Push(SqStack &S,ElemType x){
	if(S.top == MaxSize-1) //若栈满，报错
		return false ;
	S.data[++S.top] = x;   //指针先加1，再入栈
	return true;
}
````

(4)出栈
```c
bool Pop(SqStack &S,ElemType x){
	if(S.top == -1)		//栈空报错
		return false;
	x = S.data[S.tope--];   //先出栈，指针再减1
	return ture ;
}
```

(5)读栈顶元素
```c
bool GetTop(SqStack S,Elemtype &x){
	if (S.top == -1)	//栈空报错
		return false;
	x = S.data[S.top];      //栈顶元素赋值给x
	return true;

}
```

注：这里栈顶指针初始化 `S.top=-1` ，即栈顶指针指向的就是栈顶元素，故进栈时候的操作是`S.data[++S.top]=x`;出栈操作是`x = S.data[S.top--]`

如果栈顶指针初始化`S.top = 0`，即栈顶指针指向的栈顶元素的下一个元素，则入栈操作变为`S.data[S.top++]=x`；出栈操作是`x = S.data[--S.top]`

3. 共享栈
利用栈底位置相对不变的特性，可以让两个顺序栈共享一个一维数据空间，将两个栈的栈底分别设置在共享空间的两端，两个栈顶向共享空间的中间延伸。

栈的链式存储

链栈没有头结点，Lhead指向栈顶元素

```c
typedef struct Linknode{
	ElemType data;			//数据域
	Struct Linknode *next;		//指针域
} *LiStack;				//栈类型定义
```

## 队列
### 基本概念
1. 队列的定义
队列（Queue）：是一种操作受限的线性表，只允许在表的一端进行插入，而在表的另一端进行删除。
插入称为入队或者进队；
删除称为出队或者离队；

FIFO先进先出
队头（Front）：允许删除的一端
队尾（Rear）：允许插入的一端
空队列：不含任何元素

2. 队列常见的基本操作
- InitQueue(&Q):初始化队列，构造一个空队列
- QueueEmpty(Q):判队列空，为空返回true，否则返回false
- EnQueue(&Q,x):入队，若队列Q未满，将x加入到新的队尾
- DeQueue(&Q,&x):出队，若队列Q非空，删除队头元素，并用x返回
- GetHead(Q,&x):读取队头元素，若队列Q非空，则将队头元素赋值给x

注：不可以随便读取队列中间的某个数据

### 队列的顺序结构存储
1. 队列的顺序存储
队列的顺序存储是指分配一块连续的存储单元存放队列的元素，并附设两个指针front和rear分别指示队头和队尾元素的位置。

设队头指针front指向队头元素，队尾指针rear指向队尾元素的下一个位置

队列的顺序存储类型可以描述为
```c
#define MaxSize 50
typedef struct(
ElemType data[MaxSize];
int front ,rear;
}
```
初始状态（判断队空条件）Q.front == Q.rear == 0
进队操作：队不满时，先送值到队尾元素，再将队尾指针加1 
出队操作：队不空时，先取队头元素值，再将队头指针加1

注：队尾Q.rear== MaxSize不能作为队列满的条件。
队列中只有一个元素时候，仍然满足该条件。
但是入队会出现“上溢出”，但这种溢出并不是真正的溢出，在数组中仍然存在可以存放元素的位置，所以是一种假溢出

2. 循环队列

将顺序队列臆造为一个环装的空间，即把存储队列元素的表从逻辑上看做是一个环，称为循环队列。

当队首指针Q.front = MaxSize -1 后，再前进一个位置就自动到0，这时候可以利用取余运算（%）来实现。

初始时：Q.front = Q.rear = 0
队首指针进1 ：Q.front = (Q.front+1)%MaxSize
队尾指针进1：Q.rear = (Q.rear+1)%MaxSize
队列长度：(Q.rear+MaxSize-Q.front)%MaxSize
出队入队时：指针都按顺时针方向进1

注：显然队空的条件是Q.front == Q.rear

如果入队元素的速度快于出队元素的速度，队尾指针很快就赶上了队首指针。此时可以看出队满也有Q.front == Q.rear
为了区分队空还是队满，有三种处理方式：
1）牺牲一个单元来区分队空和队满，入队时少用一个队列单元。约定“队头指针在队尾指针的下一位置作为队满的标志”

队满条件：(Q.rear+1)%MaxSize == Q.front
队空条件仍为：Q.front == Q.rear
队列中元素的个数：(Q.rear - Q.front + MaxSize)%MaxSize

2)类型中增设表示元素表示个数的数据成员。
这样，队空的条件为Q.Size==0
队满的条件为Q.size==MaxSize
这两种情况都有Q.front = Q.rear

3)类型中增设tag成员，以区分是队满还是队空。
if tag= 0 因删除导致 Q.front == Q.rear 则为队空
if tag= 1 因插入导致 Q.front == Q.rear 则为队满

3. 循环队列的操作
初始化
```c
void InitQueue(&Q){
		Q.rear = Q.front = 0;//初始化队首，队尾指针
}
```
判空
```c
bool isEmpty(Q){
if (Q.rear == Q.front)//队空条件
	return ture
else 
	return false;
}
```

入队
```c
bool EnQueue(SqQueue &Q,ElemType x){
	if ((Q.rear +1)%MaxSize==Q.front)//队满
		return false
	Q.data[Q.rear+1]%MaxSize;// 队尾指针+1取模
	return true
}
```
出队
```c
bool DeQueue(SqQueue &Q, ElemType &x){
	if (Q.rear == Q.front) //队空
		return false;
	x = Q.data[Q.front];
	Q.front = (Q.front+1)%MaxSize;//队头指针+1取模
	return true

}

```

### 队列的链式存储

1. 队列的链式存储
一个同时带有队头指针和队尾指针的单链表。头指针指向队头结点，尾指针指向队尾结点，即单链表的最后一个结点（与顺序存储不同）

队列的链式存储类型可描述为
```
typedef struct{
	ElemType data;
	struct LinkNode * next;
}LinkNode;
```
Q.front == NULL 且 Q.rear = NULL时，链式队列为空

出队时，首先判断是否为空，若不空，则取出队头元素，将其从链表中摘除，并让Q.front指向下一个结点（若该结点为最后一个结点，则置Q.front和Q.rear都是NULL）入队时，建立一个新结点，将新结点插入到链表的尾部，并该让Q.rear指向这个新插入的结点（若原队列为空队列，则令Q.front也指向该结点）

将链式队列设计成一个带头结点的单链表，这样插入和删除操作就统一了。

用单链表表示的链式队列特别适合与数据元素变动比较大的情形，而且不存在队列满且产生溢出的问题。多个队列最好使用多个队列

2. 链式队列基本操作
初始化
```c
void InitQueue(LinkQueue &Q){
		Q.front = Q.rear = (LinkNode*)malloc(sizeof(LinkNode));// 建立头结点
		Q.front -> next = NULL; // 初始为空
}
```
判空
```c
bool IsEmpty(LinkQueue &Q){
	if (Q.front == Q.rear)//队空条件
		return ture
	else 
		return false;
}
```

入队
```c
void EnQueue(LinkQueue &Q,ElemType x){
	s = (LinkNode *)malloc(sizeof(LinkNode));
	s -> data = x;
	s -> next = NULL;
	Q.rear -> next = s;
	Q.rear = s;
}
```
出队
```c
bool DeQueue(LinkQueue &Q, ElemType &x){
	if (Q.front == Q.rear) //队空
		return false;
	p = Q.front -> next ;
	if (Q.rear == p)
		Q.rear ==Q.front;//若原队列中只有一个结点，删除后变空
	free (p);
	return true

}

```
### 双端队列

双端队列是运行路段都可以进行出队和入队操作的队列。其元素的逻辑结构仍然是线性结构。

前端进的排在后端进的前面。

出队时，不管前端后端出队，先出的一定排列在后出的元素的前面

输出受限的双端队列：允许一端进行插入和删除，但另一端只允许插入的双端队列。

输入受限的双端队列：允许一端进行插入和删除，但另一端只允许删除的双端队列。

- 输入受限的两端队列，假设end1端输入1,2,3,4那么


### 栈和队列的应用

3. 栈在递归中的应用

> 学习秘籍：要理解递归，你先要理解递归，直到你能理解递归。

正如秘籍中所言，递归是一种重要的程序设计方法。简单的说就是如果在一个函数、过程或者数据结构的定义中又应用了它自身，那么这个函数、过程或数据结构称为是递归定义的，简称递归。

递归通常把一个大型复杂的问题，层层转化为一个与原问题相似的规模较小的问题来求解，递归的策略的只需要少量的代码就可以描述解题过程需要的多次重复计算，大大地减少了程序的代码量。但在通常情况下，他的效率并不是太高。

以`累计求和、累计乘积、斐波那契数列``为例来看一下递归的程序实现：


`C语言`
```c
int Fib(n){
	if (n==0)             //边界条件
        return 0;
    else if(n==1)         //边界条件
        return 1;
    else
        return Fib(n-1)+Fib(n-2); //递归表达式
}

```

`Python版本`

```python
def Fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1    
    else :
        return Fib(n-1)+Fib(n-2) 
```

# 4. 树与二叉树
##  树的基本概念

## 二叉树的概念
### 二叉树的定义及其主要特性

1. 二叉树的定义

二叉树是一种树形结构，每个结点至多只有二棵子树，并且二叉树的子树有左右之分，其次序不能任意颠倒。

二叉树与度为2的有序树的区别：

- ① 度为2的树至少有3个结点，而二叉树可以为空；
- ② 度为2的有序树的孩子结点的左右次序是相对于另一孩子结点而言的，如果某个结点只有一个孩子结点，
这个孩子结点就无须区分其左右次序，而二叉树无论其孩子数是否为2，均需确定其左右次序，
也就是说二叉树的结点次序不是相对于另一结点而言的，而是确定的。


2. 几个特殊的二叉树

    - 满二叉树：高度H，含有 $2^{h}-1 $
    - 完全二叉树:
    - 二叉排序树
    - 平衡二叉树


3. 二叉树的性质：

1) 非空二叉树上叶子结点数等于度为2 的结点数加一即 $ N_{0}=N_{2}+1 $

2) 非空二叉树第K层上至多有 $2^{k-1}$ 个结点

3) 高度为H的二叉树至少有 $ 2^{H}-1 $ 个结点

4）对完全二叉树按照从上到下、从左到右的顺序依次编号，则有以下关系：
- ① 当i>1时，结点i的双亲结点的编号为 `⌊i/2⌋`
     当i为偶数时，双亲结点的编号为 `i/2`,它是双亲结点的左孩子；
     当i为奇数时，双亲结点的编号为 `⌊(i-1)/2⌋`,它是双亲结点的右孩子。
- ② 当2i<=N时，结点i的左孩子编号为 `2i`，否则无左孩子
- ③ 当2i+1<=n时，结点i的右孩子编号为 `2i+1`,否则无右孩子
- ④ 结点i所在层次(深度)为`⌊log2i⌋+1`

5）具有N个结点的完全二叉树的高度为`⌈log2(N+1)⌉`或者`⌊log2(N)⌋+1`


### 二叉树的存储结构

1. 顺序存储结构

自上而下，自左往右。

例子：

```
  └─①
	├─②
	│ ├─〇
	│ │  ├─〇
	│ │  └─〇
	│ └─④
	│    ├─⑥
	│    └─〇
	└─③
	  ├─〇
	  └─⑤
```

顺序：1-2-3-0-4-0-5-0-0-6-0	  

2. 链式存储结构

左指针lchild | 数据域 data | 右指针rchild

链式存储结构


```
  └─①
	├─l─②
	│ ├─l─〇
	│ │  ├─l─〇
	│ │  └─r─〇
	│ └─r─④
	│    ├─l─⑥
	│    └─r─〇
	└─r─③
	  ├─l─〇
	  └─r─⑤
```


## 二叉树的遍历和线索二叉树

### 二叉树的遍历

1. 先序遍历（PreOrder）
访问根结点——>先序遍历左子树——先序遍历右子树

```c
void PreOrder(Bitree T){
	if(T!=NULL){
		visit(T);
		PreOrder(T->lchild);
		PreOrder(T->rchiid);
	}
}
```


例子：
```
  └─①
	├─②
	│ ├─〇
	│ │  ├─〇
	│ │  └─〇
	│ └─④
	│    ├─⑥
	│    └─〇
	└─③
	  ├─〇
	  └─⑤	  
```
PreOrder:1-2-4-6-3-5	  
	  
2. 中序遍历（PreOrder）
中序遍历左子树——>访问根结点——>中序遍历右子树
```c
void InOrder(Bitree T){
	if(T!=NULL){
		InOrder(T->lchild);
		visit(T);
		InOrder(T->rchiid);
	}
}
```
例子：
```
  └─①
	├─②
	│ ├─〇
	│ │  ├─〇
	│ │  └─〇
	│ └─④
	│    ├─⑥
	│    └─〇
	└─③
	  ├─〇
	  └─⑤	  
```
InOrder:2-6-4-1-3-5
	  
3. 后序遍历（PreOrder）
后序遍历左子树——>后序遍历右子树——>访问根结点
```c
void PostOrder(Bitree T){
	if(T!=NULL){
		PostOrder(T->lchild);
		PostOrder(T->rchiid);
		visit(T);
	}
}
```
例子：
```
  └─①
	├─②
	│ ├─〇
	│ │  ├─〇
	│ │  └─〇
	│ └─④
	│    ├─⑥
	│    └─〇
	└─③
	  ├─〇
	  └─⑤	  
```
InOrder:6-4-2-3-5-1

记忆秘诀：
前序（访问根结点顺序在第一个）
中序（访问根结点顺序在第二个）
后序（访问根结点顺序在第三个）

4. 递归算法和非递归算法的转换

二叉树遍历算法的应用
创建二叉树
前序、中序、后序遍历。将二叉树的空结点用#标识
求二叉树中结点总数
```c

```

探究2：查询二叉树中某个结点
探究3：求二叉树的高度
拓展：求二叉树的叶子节点数
拓展：求二叉树的左右子树


5. 层次遍历 

从上到下，从左到右，一层层遍历。

6. 由遍历序列构造二叉树



### 4.3 线索二叉树

1. 线索二叉树基本概念

N个结点的二叉链表，每个结点都有指向左右孩子的结点指针，共有2N个指针，N-1个分支，N+1个空指针。

```

结点数 = 分支数 + 1（其中，结点数 = N0 + N1 + N2 ,分支数 = N1 + 2N2)
故：N0 + N1 + N2 = N1 + 2N2 + 1; 结点数 = 度为2的结点数加1 
即：N0 = N2 +1
```

2. 线索二叉树的构造


3. 线索二叉树的遍历
### 4.4 树、森林
### 1 树的存储结构：顺序存储、链式存储均可。
1. 双亲表示法：层次遍历顺序
2. 孩子表示法
3. 孩子兄弟表示法：结点值、指向第一个孩子结点的指针和指向结点的下一个兄弟结点的指针。
### 2. 树、森林与二叉树的转换
### 3. 树和森林的遍历
### 4. 并查集
### 5. 树与二叉树的应用
1. 二叉排序树（BST）二叉查找树
左子树非空，则左子树上的所有结点关键字值均小于根结点的关键字值
右子数非空，则右子数上的所有结点关键字值均大于根结点的关键字值
左右子树本省也分别是一课二叉排序树
二叉排序树是一棵递归的数据结构，可以方便






哈夫曼树

## 5. 图
### 1. 基本概念
### 2. 存储操作
邻接矩阵
邻接表
十字链表

### 3. 图的遍历
- 深度优先DFS

- 广度优先BFS
层次遍历,从上往下，从左到右
### 4. 图的应用
#### [最小生成树](https://www.douban.com/note/697347503/)

- Prime算法:找最小，依次连
- Kruskal算法:两两连接最小边

#### 最短路径

- Dijkstra算法：并查集与子集
- Floyd算法：更新邻接矩阵

#### 拓扑排序
- 依次去掉出发结点
#### 关键路径
```
Ve(i):头结点到各个结点的最大距离，比如Ve(4)=a2+a5=2+4=6

Vl(i):倒序写，依次用最大距离减去临近的边长（注意最近的问题）比如：Vl(3)=V4-a5=6-4=2

e(i):将Ve(i)按结点划分写（最后一个结点不用写）

V1 | V2 | V3 | V4 | V5

a1 a2 | a3 a4 | a5 a6 | a7 | a8

0 0 3 3 2 2 6 6    

l(i):最近一个结点减去边长，例l(5) = V4-4

l-e=0为关键结点
```

## 6. 查找

### 查找的基本概念

### 顺序查找的折半查找
时间复杂度
### B树和B+树
- 构建B树
- B树的性质
- B树的操作

- 构建B+树
- B+树的性质


### 7. 排序
排序的定于及基础概念
- 插入排序
	直接插入排序
	折半排序
	希尔排序
	
- 交换排序
	冒泡排序
	快速排序
- 选择排序 简单选择排序 堆排序
- 归并排序
- 基数排序
