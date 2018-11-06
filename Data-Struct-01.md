# Data-Struct
# 线性表
_A1->A2->……->An_

## 逻辑结构
1. 定义：线性表是相同数据类型的数据元素的有线序列。
2. 特点
- 相同特性：方便用统一的方法处理
- 有限：表中元素的个数是N
- 序列：表中元素排成一列，表中元素除第一个元素外有且仅有一个前驱，除最后一个元素外有且仅有一个后继。

## 存储结构：

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
2018-10-31
09.00-10.00 data struct
10.00-11.00 English
11.00-11.30 dahua
11.30-12.00 search word


### 3.5.3 头指针与头结点的异同

- 头指针
	- 头指针是指链表指向第一个结点的指针，若链表有头结点，则是指向头结点的指针
	- 头指针具有标识作用，所以常用指针冠以链表的名字
	- 无论链表是否为空，头指针均不为空，**头指针是链表的必要元素**。

- 头结点
	- 头结点是为了操作的统一和方便而设立的，放在第一元素的结点之前，其数据域一般无意义（也可存放链表的长度）
	- 有了头结点，对在第一元素结点前插入结点和删除第一结点，其操作与其它结点的操作就统一了
	- **头结点不一定是链表必要元素**。
