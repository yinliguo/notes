## Data Structure
### 线性表
```
ADT List {
  数据对象: D = {a[i] | a[i] ∈ ElemSet, i = 1,2,...,n, n≥0}
  数据关系: R1 = {<a[i-1], a[i]>|a[i-1], a[i] ∈ D, i = 1,2,...,n}
  基本操作:
    InitList(&L)
      操作结果：构造一个空的线性表L
    DestoryList(&L)
      初始条件：线性表L已存在
      操作结果：销毁线性表L
    ClearList(&L)
      初始条件：线性表L已存在
      操作结果：将L重置为空表
    ListEmpty(&L)
      初始条件：线性表L已存在
      操作结果：若L为空表，返回TRUE，否则返回FALSE
    ListLength(&L)
      初始条件：线性表L已存在
      操作结果：返回L中数据元素的个数
    GetElem(L, i, &e)
      初始条件：线性表L已存在，1 ≤ i ≤ ListLength(L)
      操作结果：用e返回L中第i个元素的值
    LocateElem(L, e, compare())
      初始条件：线性表L已存在，compare是数据元素判定的函数
      操作结果：返回L中第一个与e满足关系compare()的数据元素位序。若这样的元素不存在，则返回值为0
    PriorElem(L, cur_e, &pre_e)
      初始条件：线性表L已存在
      操作结果：若cur_e是L的数据元素，且不是第一个，则用pre_e返回它的前驱，否则操作失败，pre_e无定义
    NextElem(L, cur_e, &next_e)
      初始条件：线性表已存在
      操作结果：若cur_e是L的数据元素，且不是最后一个，则用next_e返回它的后继，否则操作失败，next_e无定义
    ListInsert(&L, i, e)
      初始条件：线性表L已存在，1 ≤ i ≤ ListLength(L) + 1
      操作结果：在L中第i个位置之前插入新的数据元素e，L的长度加1
    ListDelete(&L, i, e)
      初始条件：线性表L已存在且非空，1 ≤ i ≤ ListLength(L)
      操作结果：删除L的第i个数据元素，并用e返回其值，L的长度减1
    ListTraverse(L, visit())
      初始条件：线性表L已存在
      操作结果：依次对L的每个数据元素调用函数visit()，一旦visit()失败，则操作失败。
} ADT List
```
> 线性表有2中表示方式，一种是顺序表示，一种是链式表示

#### 1、顺序存储
顺序表示是指用一组地址连续的存储单元依次存储线性表的数据元素，可以进行随机存取，但是插入或删除时需要移动大量元素
#### 2、链式存储
##### 2.1、线性链表

```
type node struct {
  next  *node
  value interface
}
```
* 插入和删除只需要找到要对应的元素通过修改指针就可以了，时间复杂度为O(n)。但不能像顺序表示那样随机读取。
* 遍历时判断next为空为终止条件

##### 2.2、循环链表
```
type node struct {
  next  *node
  value interface
}

```
* 结构与线性链表一样，但最后一个元素的next指向第一个元素
* 循环遍历的结束条件是判断next是否为第一个元素的地址
* 如果有尾指针，合并链表会更方便
* 线性链表和循环链表取下一个元素的时间复杂度为O(1)，但是取上一个元素时间复杂度为O(n)，因此产生了双向链表

##### 2.3、双向链表
```
type node struct {
  prev  *node
  next  *node
  value interface
}
```
* 双向链表也可以有循环表

### 栈
栈是限定仅在表尾进行插入和删除的线性表
```
ADT Stack {
  数据对象: D = {a[i] | a[i] ∈ ElemSet, i = 1,2,...,n, n ≥ 0}
  数据关系: R1 = {<a[i-1], a[i]> | a[i-1], a[i] ∈ D, i = 1,2,...,n, n ≥ 0}
           约定a[n]为栈顶，a[1]为栈底
  基本操作:
    InitStack(&S)
      操作结果：构造一个空栈S
    DestoryStack(&S)
      初始条件：栈S已存在
      操作结果：栈S被销毁
    ClearStack(&S)
      初始条件：栈S已存在
      操作结果：将栈S清为空栈
    StackEmpty(S)
      初始条件：栈S已存在
      操作结果：若栈S为空栈，返回TRUE，否则返回FALSE
    StackLength(S)
      初始条件：栈S已存在
      操作结果：返回S的元素个数，即栈的长度
    GetTop(S, &e)
      初始条件：栈S已存在且非空
      操作结果：用e返回S的栈顶元素
    Push(&S, e)
      初始条件：栈S已存在
      操作结果：插入元素e为新的栈顶元素
    Pop(&S, &e)
      初始条件：栈S已存在且非空
      操作结果：删除S的栈顶元素，并用e返回其值
    StackTraserve(S, visit())
      初始条件：栈S已存在且非空
      操作结果：从栈底到栈顶依次对S的每个数据元素调用函数visit()。一旦visit()失败，则操作失败
}ADT Stack
```
栈也可以使用顺序表示和链式表示
#### 1、顺序表示
由于栈的大小不知道，所以预先分配一个基本容量，不够了再扩展
#### 2、链式表示

### 队列
队列是一种先进先出的线性表，允许再一端插入，另一端删除元素。
> 除了栈和队列外，还有一种限定性数据结构是*双端队列*(dequeue)，限定两端都可以进行插入和删除。虽然看起来很酷，但实际应用场景不多。

```
ADT Queue {
  数据对象: D = {a[i] | a[i] ∈ ElemSet, i = 1,2,...,n, n ≥ 0}
  数据关系: R1 = {<a[i-1], a[i]> | a[i-1], a[i] ∈ D, i = 2,...,n}
  基本操作:
    InitQueue(&Q)
      操作结果：构造一个空队列Q
    DestoryQueue(&Q)
      初始条件：队列Q已存在
      操作结果：队列Q被销毁，不再存在
    ClearQueue(&Q)
      初始条件：队列Q已存在
      操作结果：将Q清为空队列
    QueueEmpty(Q)
      初始条件：队列Q已存在
      操作结果：若Q为空队列，返回TRUE，否则返回FALSE
    QueueLength(Q)
      初始条件：队列Q已存在
      操作结果：返回Q的元素个数，即队列的长度
    GetHead(Q, &e)
      初始条件：Q为非空队列
      操作结果：用e返回Q的队头元素
    EnQueue(&Q, e)
      初始条件：队列Q已存在
      操作结果：插入e为Q的新的队尾元素
    DeQueue(&Q, &e)
      初始条件：Q为非空队列
      操作结果：删除Q的队头元素，并用e返回其值
    QueueTraserve(Q, visit())
      初始条件：Q已存在且非空
      操作结果：从队头到队尾，依次对Q的每个数据元素调用函数visit()。一旦visit()失败，则操作失败。
}ADT Queue
```
#### 队列的链式表示
用链表表示的队列简称为*链队列*
```
type node struct {
  next  *node
  value interface
}
type queue struct {
  front *node
  rear  *node
}
```

#### 队列的顺序表示
可以使用一个一维数组来构造一个*循环队列*，缺点是不能应付未知长度的队列
```
type node struc {
  value interface
}
type queue struct {
  base  *node
  front *node
  rear  *ndoe
}
```

### 串
### 数组
### 广义表
### 树

