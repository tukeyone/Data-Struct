> 余幼时即嗜学。家贫，无从致书以观，每假借于藏书之家，手自笔录，计日以还。天大寒，砚冰坚，手指不可屈伸，弗之怠。录毕，走送之，不敢稍逾约。以是人多以书假余，余因得遍观群书。既加冠，益慕圣贤之道。又患无硕师名人与游，尝趋百里外，从乡之先达执经叩问。先达德隆望尊，门人弟子填其室，未尝稍降辞色。余立侍左右，援疑质理，俯身倾耳以请；或遇其叱咄，色愈恭，礼愈至，不敢出一言以复；俟其欣悦，则又请焉。故余虽愚，卒获有所闻。

# Data-Struct
# 线性表
## 逻辑结构
1. 定义：线性表是相同数据类型的数据元素的有线序列。
2. 特点
- 相同特性：方便用统一的方法处理
- 有限：表中元素的个数是N
- 序列：表中元素排成一列，表中元素除第一个元素外有且仅有一个前驱，除最后一个元素外有且仅有一个后继。
## 存储结构
### 顺序存储-顺序表



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
```
L.data = (ElemType *)malloc(InitSize*sizeof(ElemType));
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
---


### 链式存储-链表
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
# 栈和队列
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

































































2018-10-31
09.00-10.00 data struct
10.00-11.00 English
11.00-11.30 dahua
11.30-12.00 search word
