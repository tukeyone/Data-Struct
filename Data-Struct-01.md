# Data-Struct
# 线性表
A1->A2->……->An

## 逻辑结构
1. 定义：线性表是相同数据类型的数据元素的有线序列。
2. 特点
- 相同特性：方便用统一的方法处理
- 有限：表中元素的个数是N
- 序列：表中元素排成一列，表中元素有且仅有一个前驱和一个后继

## 存储结构：

### 顺序存储

```c
# define maxSize 50
int A[maxSize];
int length = 0;
//知道了第一个位置以及房间个数就知道任意房间位置
//0<length<maxSize
```

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
