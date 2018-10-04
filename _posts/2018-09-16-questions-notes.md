---
layout: post
title:  "Questions"
date:   2018-09-17 11:41:00 +0800
featured-img: questions
categories: Questions
summary: These are some of the questions I encountered while doing exercises, and some related knowledge points.
---

![](/assets/img/posts/questions/Snipaste_2018-09-17_14-57-30.png)

![](/assets/img/posts/questions/Snipaste_2018-09-17_14-52-59.png)

![](/assets/img/posts/questions/Snipaste_2018-09-17_14-49-58.png)

java中小数默认为double类型

![](/assets/img/posts/questions/Snipaste_2018-09-17_14-44-25.png)

因为100%4=0，1000%8=0，10000%16=0，所以845678992357836701%16 = 6701%16，然后因为2000%16=0，所以6701%16 = 701%16 = 13 =D

![](/assets/img/posts/questions/Snipaste_2018-09-17_15-16-47.png)

![](/assets/img/posts/questions/Snipaste_2018-09-16_21-57-40.png)

![](/assets/img/posts/questions/Snipaste_2018-09-16_21-58-17.png)

![](/assets/img/posts/questions/Snipaste_2018-09-16_21-59-52.png)

![](/assets/img/posts/questions/Snipaste_2018-09-16_22-01-28.png)

![](/assets/img/posts/questions/Snipaste_2018-09-16_22-02-16.png)

Object类中的所有方法

![](/assets/img/posts/questions/Snipaste_2018-09-22_13-00-16.png)

![](/assets/img/posts/questions/Snipaste_2018-09-17_15-50-18.png)

![](/assets/img/posts/questions/Snipaste_2018-09-17_11-48-56.png)

### 排序

#### 冒泡排序

一趟排序过程

![](/assets/img/posts/questions/Snipaste_2018-09-18_16-40-42.png)![](/assets/img/posts/questions/Snipaste_2018-09-18_16-41-28.png)![](/assets/img/posts/questions/Snipaste_2018-09-18_16-42-30.png)![](/assets/img/posts/questions/Snipaste_2018-09-18_16-43-04.png)

最好情况是整个序列从一开始就是有序的，但至少要从上到下扫描一遍，所以时间复杂度是O(N),最坏情况是序列一开始是完全逆序的，每一趟都要两两交换，且要扫描N趟，所以时间复杂度是O(N^2).冒泡排序的一个好处是不管对于数组存储的数据还是链表存储的数据都是没有问题的，因为它每次都是从上到下按顺序扫描，而且交换的是相邻的元素。

![](/assets/img/posts/questions/Snipaste_2018-09-18_16-54-34.png)

#### 插入排序

![](/assets/img/posts/questions/Snipaste_2018-09-18_17-57-10.png)![](/assets/img/posts/questions/Snipaste_2018-09-18_17-57-42.png)![](/assets/img/posts/questions/Snipaste_2018-09-18_17-58-05.png)![](/assets/img/posts/questions/Snipaste_2018-09-18_17-59-06.png)![](/assets/img/posts/questions/Snipaste_2018-09-18_17-59-31.png)![](/assets/img/posts/questions/Snipaste_2018-09-18_18-00-47.png)![](/assets/img/posts/questions/Snipaste_2018-09-18_18-01-29.png)

![](/assets/img/posts/questions/Snipaste_2018-09-18_18-07-58.png)

![](/assets/img/posts/questions/Snipaste_2018-09-18_18-08-13.png)

插入排序的最好情况也是原来的序列就是有序的，此时原有的牌不需要向后错位，新牌直接插在原来的牌后面，此时的时间复杂度是O(N),原序列是逆序时复杂度为O(N^2),因为每插入一张新牌都要将之前的所有牌向后错一位。

#### 快速排序

选主元

![](/assets/img/posts/questions/Snipaste_2018-09-19_09-55-52.png)

For Example:

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-20-22.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-22-43.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-29-19.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-37-12.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-40-25.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-45-27.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-46-29.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-47-50.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-49-02.png)

......

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-50-04.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-51-24.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-52-31.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-53-33.png)

......

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-54-43.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-55-51.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-56-54.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-59-22.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_10-58-11.png)



现在有调用完`Median3`函数之后的十个数字，所以最左边和最右边的元素都不用考虑了，因为最左边的那个元素肯定比6小，最右边的那个元素肯定比6大。所以在图上没有显示那两个元素了。

接下来我们要找之前选定的主元(也就是6)应该被放在哪里。首先，我们定义一个左边的指针，叫做`i`，再定义一个右边的指针，叫做`j`。注意，这里的指针并不是C语言里的指针，他们只是代表元素在数组里的位置，只是一个数组的下标，所以定义成整数就可以了。接着先将`i`所指元素跟主元比较，然后我们在8上面添加一个红色的符号，我们发现它是不对的，因为我们需要的应该是左边的元素全部小于等于6，右边的元素全部大于等于6。然后`i`就不动了，指在那里。然后考虑`j`，`j`指向7，大于6，没问题，`j--`,往左挪，指向2，但2是小于6的，所以添加红色符号，又不对了。在两边都发现了有不对的元素以后，我们就需要把两个元素交换一下，接着重复以上过程进行下一轮比较，直到i越过了j，即`i-j<0`。此时，意味着这趟划分结束了，最后一步也就是将`i`位置的元素和主元进行交换，把6放到它应该放的位置。

快速排序之所以快的原因就是它的主元被选中以后，在子集划分完成之后，它被一次性地放到了正确的位置上，以后再也不会去动。

如果有元素和主元相等，停下来交换。

由于快速排序是用递归的，所以对于大规模的数据我们用递归，对于小规模的数据，直接调用简单排序(如插入排序)，我们可以在程序中定义一个Cutoff的阈值。

```c
/* 快速排序 */

ElementType Median3( ElementType A[], int Left, int Right )
{
    int Center = (Left+Right) / 2;
    if ( A[Left] > A[Center] )
        Swap( &A[Left], &A[Center] );
    if ( A[Left] > A[Right] )
        Swap( &A[Left], &A[Right] );
    if ( A[Center] > A[Right] )
        Swap( &A[Center], &A[Right] );
    /* 此时A[Left] <= A[Center] <= A[Right] */
    Swap( &A[Center], &A[Right-1] ); /* 将基准Pivot藏到右边*/
    /* 只需要考虑A[Left+1] … A[Right-2] */
    return  A[Right-1];  /* 返回基准Pivot */
}

void Qsort( ElementType A[], int Left, int Right )
{ /* 核心递归函数 */
     int Pivot, Cutoff, Low, High;

     if ( Cutoff <= Right-Left ) { /* 如果序列元素充分多，进入快排 */
          Pivot = Median3( A, Left, Right ); /* 选基准 */
          Low = Left; High = Right-1;
          while (1) { /*将序列中比基准小的移到基准左边，大的移到右边*/
               while ( A[++Low] < Pivot ) ;
               while ( A[--High] > Pivot ) ;
               if ( Low < High ) Swap( &A[Low], &A[High] );
               else break;
          }
          Swap( &A[Low], &A[Right-1] );   /* 将基准换到正确的位置 */
          Qsort( A, Left, Low-1 );    /* 递归解决左边 */
          Qsort( A, Low+1, Right );   /* 递归解决右边 */  
     }
     else InsertionSort( A+Left, Right-Left+1 ); /* 元素太少，用简单排序 */
}

void QuickSort( ElementType A[], int N )
{ /* 统一接口 */
     Qsort( A, 0, N-1 );
}
```

#### 堆排序

![](/assets/img/posts/questions/Snipaste_2018-09-19_11-49-31.png)

```c
typedef struct HNode *Heap; /* 堆的类型定义 */
struct HNode {
    ElementType *Data; /* 存储元素的数组 */
    int Size;          /* 堆中当前元素个数 */
    int Capacity;      /* 堆的最大容量 */
};
typedef Heap MaxHeap; /* 最大堆 */
typedef Heap MinHeap; /* 最小堆 */

#define MAXDATA 1000  /* 该值应根据具体情况定义为大于堆中所有可能元素的值 */

MaxHeap CreateHeap( int MaxSize )
{ /* 创建容量为MaxSize的空的最大堆 */

    MaxHeap H = (MaxHeap)malloc(sizeof(struct HNode));
    H->Data = (ElementType *)malloc((MaxSize+1)*sizeof(ElementType));
    H->Size = 0;
    H->Capacity = MaxSize;
    H->Data[0] = MAXDATA; /* 定义"哨兵"为大于堆中所有可能元素的值*/

    return H;
}
```

最大堆的插入

![](/assets/img/posts/questions/Snipaste_2018-09-19_12-02-06.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_12-03-09.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_12-07-31.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-12-16.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-15-51.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-23-59.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-27-49.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-31-40.png)

```c
bool IsFull( MaxHeap H )
{
    return (H->Size == H->Capacity);
}

bool Insert( MaxHeap H, ElementType X )
{ /* 将元素X插入最大堆H，其中H->Data[0]已经定义为哨兵 */
    int i;

    if ( IsFull(H) ) {
        printf("最大堆已满");
        return false;
    }
    i = ++H->Size; /* i指向插入后堆中的最后一个元素的位置 */
    for ( ; H->Data[i/2] < X; i/=2 )
        H->Data[i] = H->Data[i/2]; /* 上滤X */
    H->Data[i] = X; /* 将X插入 */
    return true;
}
```

最大堆的删除

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-39-54.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-41-27.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-43-45.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-45-14.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_13-47-07.png)

```c
bool IsEmpty( MaxHeap H )
{
    return (H->Size == 0);
}

ElementType DeleteMax( MaxHeap H )
{ /* 从最大堆H中取出键值为最大的元素，并删除一个结点 */
    int Parent, Child;
    ElementType MaxItem, X;

    if ( IsEmpty(H) ) {
        printf("最大堆已为空");
        return ERROR;
    }

    MaxItem = H->Data[1]; /* 取出根结点存放的最大值 */
    /* 用最大堆中最后一个元素从根结点开始向上过滤下层结点 */
    X = H->Data[H->Size--]; /* 注意当前堆的规模要减小 */
    for( Parent=1; Parent*2<=H->Size; Parent=Child ) {
        Child = Parent * 2;
        if( (Child!=H->Size) && (H->Data[Child]<H->Data[Child+1]) )
            Child++;  /* Child指向左右子结点的较大者 */
        if( X >= H->Data[Child] ) break; /* 找到了合适位置 */
        else  /* 下滤X */
            H->Data[Parent] = H->Data[Child];
    }
    H->Data[Parent] = X;

    return MaxItem;
}
```

最大堆的建立

![](/assets/img/posts/questions/Snipaste_2018-09-19_14-55-44.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_14-57-19.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_14-58-27.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_14-59-36.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-00-31.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-01-26.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-02-32.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-09-34.png)

```c
/*----------- 建造最大堆 -----------*/
void PercDown( MaxHeap H, int p )
{ /* 下滤：将H中以H->Data[p]为根的子堆调整为最大堆 */
    int Parent, Child;
    ElementType X;

    X = H->Data[p]; /* 取出根结点存放的值 */
    for( Parent=p; Parent*2<=H->Size; Parent=Child ) {
        Child = Parent * 2;
        if( (Child!=H->Size) && (H->Data[Child]<H->Data[Child+1]) )
            Child++;  /* Child指向左右子结点的较大者 */
        if( X >= H->Data[Child] ) break; /* 找到了合适位置 */
        else  /* 下滤X */
            H->Data[Parent] = H->Data[Child];
    }
    H->Data[Parent] = X;
}

void BuildHeap( MaxHeap H )
{ /* 调整H->Data[]中的元素，使满足最大堆的有序性  */
  /* 这里假设所有H->Size个元素已经存在H->Data[]中 */

    int i;

    /* 从最后一个结点的父节点开始，到根结点1 */
    for( i = H->Size/2; i>0; i-- )
        PercDown( H, i );
}
```

堆排序

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-26-15.png)

先将其调整为一个最大堆

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-29-16.png)

接着将根节点和最后一个节点进行交换

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-31-16.png)

然后将堆的规模减一，后面的操作就可以将d排除在外，因为d已经在它正确的位置上了。再继续将剩下的元素调成最大堆

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-35-25.png)

然后再重复之前的步骤。

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-37-01.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-38-14.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-39-40.png)

堆排序时第一个元素下标为零，没有像之前的堆那样有一个哨兵元素，所以在循环的判断时要注意`i>0`这个条件。

```c
void Swap( ElementType *a, ElementType *b )
{
     ElementType t = *a; *a = *b; *b = t;
}

void PercDown( ElementType A[], int p, int N )
{ /* 改编代码4.24的PercDown( MaxHeap H, int p )    */
  /* 将N个元素的数组中以A[p]为根的子堆调整为最大堆 */
    int Parent, Child;
    ElementType X;

    X = A[p]; /* 取出根结点存放的值 */
    for( Parent=p; (Parent*2+1)<N; Parent=Child ) {
        Child = Parent * 2 + 1;
        if( (Child!=N-1) && (A[Child]<A[Child+1]) )
            Child++;  /* Child指向左右子结点的较大者 */
        if( X >= A[Child] ) break; /* 找到了合适位置 */
        else  /* 下滤X */
            A[Parent] = A[Child];
    }
    A[Parent] = X;
}

void HeapSort( ElementType A[], int N )
{ /* 堆排序 */
     int i;

     for ( i=N/2-1; i>=0; i-- )/* 建立最大堆 */
         PercDown( A, i, N );

     for ( i=N-1; i>0; i-- ) {
         /* 删除最大堆顶 */
         Swap( &A[0], &A[i] ); /* 见代码7.1 */
         PercDown( A, 0, i );
     }
}
```

![](/assets/img/posts/questions/Snipaste_2018-09-19_15-51-37.png)

### 滑动窗口协议

由于Rdt3.0采用停等机制，所以其发送方利用率非常低下。因此，我们可以采用流水线机制来对其进行改进。通过流水线机制，可以允许发送方在收到ACK之前连续发送多个分组。同时，我们需要更大的序列号范围，发送方和/或接收方需要更大的空间以缓存分组。以前是发送一个分组，收到对应的ACK后就将其删除，但现在是连续发多个分组，所以就需要缓存更多还没确认的分组。而在计算机网络中实现流水线机制的便是滑动窗口协议。

![](/assets/img/posts/questions/Snipaste_2018-09-19_18-37-10.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_18-39-54.png)

图中绿色的部分表示已经发送完，并且成功确认的分组，黄色的部分表示已经发出去，但还没确认的分组，蓝色的表示还可以使用的序列号。

#### GBN(Go-Back-N)

GBN采用累计确认机制

![](/assets/img/posts/questions/Snipaste_2018-09-19_18-54-22.png)

发送方FSM

![](/assets/img/posts/questions/Snipaste_2018-09-19_20-00-42.png)

接收方FSM

![](/assets/img/posts/questions/Snipaste_2018-09-19_20-20-42.png)

#### SR(Selective Repeat)

![](/assets/img/posts/questions/Snipaste_2018-09-19_21-33-57.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_21-34-53.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_21-35-29.png)

![](/assets/img/posts/questions/Snipaste_2018-09-19_21-36-08.png)

### TCP

TCP同样采用流水线机制以及累计确认机制。但和GBN不一样的是，当发生超时事件是，只重传引起超时的segment。

#### TCP发送方事件

![](/assets/img/posts/questions/Snipaste_2018-09-20_10-40-09.png)

#### TCP发送端程序

![](/assets/img/posts/questions/Snipaste_2018-09-20_10-44-39.png)

#### TCP重传示例

![](/assets/img/posts/questions/Snipaste_2018-09-20_10-46-16.png)

![](/assets/img/posts/questions/Snipaste_2018-09-20_10-47-15.png)

#### TCP ACK生成

![](/assets/img/posts/questions/Snipaste_2018-09-20_10-54-29.png)

#### 快速重传

![](/assets/img/posts/questions/Snipaste_2018-09-20_10-56-03.png)

![](/assets/img/posts/questions/Snipaste_2018-09-20_10-57-11.png)

#### TCP流量控制

由于上层应用处理速度较慢，而如果发送方发送数据太多，就会导致接收方buffer溢出，所以需要进行流量控制。

![](/assets/img/posts/questions/Snipaste_2018-09-20_11-37-11.png)

事实上TCP receiver并不会丢弃乱序的segment。

![](/assets/img/posts/questions/Snipaste_2018-09-20_11-37-23.png)

当Recerver告知Sender RcvWindow=0时，之后发送方任然可以发送一个很小的TCP段，从而捎回来新的RcvWindow信息，以便避免死锁情况的发生。

#### TCP三次握手和四次挥手

利用TCP三次握手，可以发动syn洪水攻击(https://www.cisco.com/c/en/us/about/press/internet-protocol-journal/back-issues/table-contents-34/syn-flooding-attacks.html)

![](/assets/img/posts/questions/Snipaste_2018-09-20_13-37-23.jpg)

![](/assets/img/posts/questions/Snipaste_2018-09-20_13-52-24.png)

![](/assets/img/posts/questions/Snipaste_2018-09-20_11-37-24.png)![](/assets/img/posts/questions/Snipaste_2018-09-20_13-54-50.png)

java中，方法重写时，访问修饰符的限制一定要大于等于被重写方法的访问修饰符，子类中的方法与父类中的方法具有相同的方法名、返回类型和参数表



![](/assets/img/posts/questions/Snipaste_2018-09-22_19-55-30.png)

![](/assets/img/posts/questions/Snipaste_2018-09-22_19-56-38.png)

![](/assets/img/posts/questions/Snipaste_2018-09-22_19-58-30.png)

方法构造器没有返回类型

在MySql中

![](/assets/img/posts/questions/Snipaste_2018-09-23_15-11-24.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_15-40-58.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_15-42-44.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_15-43-06.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_16-54-28.png)

To invoke a stored procedure, use the `CALL` statement. To invoke a stored function, refer to it in an expression. The function returns a value during expression evaluation.

`CREATE PROCEDURE` and `CREATE FUNCTION` require the `CREATE ROUTINE` privilege. They might also require the `SUPER` privilege, depending on the `DEFINER` value, as described later in this section. If binary logging is enabled, `CREATE FUNCTION` might require the SUPER privilege.

By default, MySQL automatically grants the `ALTER ROUTINE` and `EXECUTE` privileges to the routine creator. This behavior can be changed by disabling the `automatic_sp_privileges` system variable.

The parameter list enclosed within parentheses must always be present. If there are no parameters, an empty parameter list of `()` should be used. Parameter names are not case sensitive.

Each parameter is an `IN` parameter by default. To specify otherwise for a parameter, use the keyword `OUT` or `INOUT` before the parameter name.

![](/assets/img/posts/questions/Snipaste_2018-09-23_18-02-19.png)

An `IN` parameter passes a value into a procedure. The procedure might modify the value, but the modification is not visible to the caller when the procedure returns. An `OUT` parameter passes a value from the procedure back to the caller. Its initial value is `NULL` within the procedure, and its value is visible to the caller when the procedure returns. An `INOUT` parameter is initialized by the caller, can be modified by the procedure, and any change made by the procedure is visible to the caller when the procedure returns.

For each `OUT` or `INOUT` parameter, pass a user-defined variable in the `CALL` statement that invokes the procedure so that you can obtain its value when the procedure returns. If you are calling the procedure from within another stored procedure or function, you can also pass a routine parameter or local routine variable as an `OUT` or `INOUT` parameter. If you are calling the procedure from within a trigger, you can also pass `NEW.col_name` as an `OUT` or `INOUT` parameter.

The following example shows a simple stored procedure that uses an OUT parameter:

```mysql
mysql> delimiter //
mysql> CREATE PROCEDURE simpleproc (OUT param1 INT)
 -> BEGIN
 -> SELECT COUNT(*) INTO param1 FROM t;
 -> END//
Query OK, 0 rows affected (0.00 sec)
mysql> delimiter ;
mysql> CALL simpleproc(@a);
Query OK, 0 rows affected (0.00 sec)
mysql> SELECT @a;
+------+
| @a |
+------+
| 3 |
+------+
1 row in set (0.00 sec)
```

Linux创建符号链接命令是`ln`,也即“link”的缩写，并加上“-s”选项，表示创建的是符号(Symbolic)链接,`ln`后跟两个参数，第一个是链接要指向的文件，第二个是要创建的链接文件名。

虽然我们可以通过符号链接来读，写和执行文件，但这完全不代表符号链接和源文件是同一个文件，源文件和其符号链接文件完全是两个不同的文件。不仅名字不一样，文件的各个属性都不同。同时，符号链接文件的名字非常特殊名字中不仅有符号链接文件名，还有目标文件名。如果符号链接文件所指向的源文件被删除了，那么这个符号链接文件就失效，读取或者执行这个符号链接文件，会显示“没有那个文件或目录”的提示。其名字也会变为醒目的红色，这种情况，我们就称这个符号链接“断裂”了。

![](/assets/img/posts/questions/Snipaste_2018-09-23_19-45-32.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_19-48-31.png)



![](/assets/img/posts/questions/Snipaste_2018-09-23_20-04-12.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-07-14.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-11-30.png)

创建硬链接和创建符号链接的命令一样，只不过不需要加上`-s`选项

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-15-41.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-13-49.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-26-45.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-31-06.png)

无论是sample.txt，sample_hl01，还是sample_hl02，本质上是同一个文件，只不过这个文件有多个名字。这一点可以通过他们的`inode-number`来进行观察验证。这三个文件的`inode-number`是一样的，充分说明了这三个文件名本质上就是同一文件。

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-34-36.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-40-22.png)

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-41-01.png)

将源文件删除以后，其他两个硬链接文件仍然可以正常打开，文件属性和内容并无异常，再删除sample_hl02文件，余下的sample_hl01文件，除了文件属性中的硬链接数目变成了1，其他一切仍然正常。我们为sample.txt文件创建了两个硬链接，那么就相当于这个文件在文件系统里有了三个文件名，这三个文件名都指向同一份文件内容，删除了一个文件，只是将某个文件名从文件系统中移除了，我们仍然可以通过余下的文件名来访问这个文件的数据。只要文件还存在着一个文件名，这个文件就一切正常，仍然可以访问。除非将这三个文件都删除。我们才无法通过文件名找到文件inode，进而访问文件数据，文件才算被真正删除掉。

![](/assets/img/posts/questions/Snipaste_2018-09-23_20-48-29.png)

由于inode 仅在特定文件系统内是惟一的，因此硬链接不能够跨越文件系统。符号链接没有上面的限制，具有更大的灵活性，甚至可以跨越不同机器、不同网络对文件进行链接。

我们不推荐为目录创建硬链接，容易造成目录遍历死循环，而且不能夸硬盘分区创建硬链接，因为在不同的分区中，文件的`inode-number`不再是唯一的了。

### http会话的四个过程

1.建立tcp连接

2.发出请求文档

3.发出响应文档

4.释放tcp链接

### 进程调度

![](/assets/img/posts/questions/Snipaste_2018-09-23_21-04-35.png)

Java 中case后必须跟常数，不可以跟变量。

![](/assets/img/posts/questions/Snipaste_2018-09-26_22-02-08.png)

You don't have to provide any constructors for your class, but you must be careful when doing this. The compiler automatically provides a no-argument, default constructor for any class without constructors. This default constructor will call the no-argument constructor of the superclass. In this situation, the compiler will complain if the superclass doesn't have a no-argument constructor so you must verify that it does. If your class has no explicit superclass, then it has an implicit superclass of `Object`, which *does* have a no-argument constructor.

If you specified a constructor explicitly (as in `Parent`), the Java compiler will *not* create a no-argument constructor for you.

If you didn't specify a constructor explicitly (as in `Child`) the Java compiler will create a no-argument constructor for you like this:

```java
public Child() {
    super();
}
```

That's trying to call the superclass no-argument  constructor - so it has to exist. You have two options:

Provide a no-argument constructor explicitly in `Parent`

Provide a no-argument constructor explicitly in `Child` which explicitly calls the base class constructor with an appropriate `int` argument like this:

```java
public Child(int variable) {
    super(variable);
}
```

![](/assets/img/posts/questions/Snipaste_2018-09-27_10-44-12.png)

![](/assets/img/posts/questions/Snipaste_2018-09-27_10-45-24.png)

### 页面置换

#### FIFO

![](/assets/img/posts/questions/Snipaste_2018-09-28_13-48-28.png)

![](/assets/img/posts/questions/Snipaste_2018-09-28_13-49-21.png)

#### Belady异常

![](/assets/img/posts/questions/Snipaste_2018-09-28_13-51-53.png)

### 最优置换算法OPT

![](/assets/img/posts/questions/Snipaste_2018-09-28_13-55-40.png)

![](/assets/img/posts/questions/Snipaste_2018-09-28_13-56-30.png)

### 最近最少使用算法LRU

![](/assets/img/posts/questions/Snipaste_2018-09-28_13-58-16.png)

![](/assets/img/posts/questions/Snipaste_2018-09-28_13-59-14.png)

### 数据结构

根据数据元素之间关系的不同特性,通常有下列四种基本结构：

1.  **集合** 结构中的数据元素之间除了“同属于一个集合”的关系外，别无其他关系。
2.  **线性结构** 结构中的数据元素之间存在一个对一个的关系。
3. **树状结构** 结构中的数据元素之间存在一个对多个的关系。
4.  **图状结构或网状结构** 结构中的数据元素之间存在多个对多个的关系。

上述数据结构的定义是对操作对象的一种数学描述，是从操作对象抽象出来的数学模型。结构定义中的“关系”描述的是数据元素的**逻辑关系**，因此又称为数据的逻辑结构。

数据元素之间的关系在计算机中又有两种不同的表示方法 ：**顺序映像**和**非顺序映像**，并由此得到两种不同的存储结构：**顺序存储结构**和**链式存储结构**。



Redis supports 5 types of data types(https://redis.io/topics/data-types):

- Strings
- Lists
- Sets
- Hashes
-  Sorted sets



```java
public class Parent {

    {
        System.out.println("parent");
    }

    static {
        System.out.println("parent static");
    }

    public Parent() {
        System.out.println("parent constructor");
    }
}
```

```java
public class Sub extends Parent {

    {
        System.out.println("sub");
    }

    public Sub() {
        System.out.println("sub constructor");
    }

    static {
        System.out.println("sub static");
    }
}
```

![](/assets/img/posts/questions/Snipaste_2018-09-28_21-54-47.png)

### 位运算（Bitwise operation）

#### 按位与&

A **bitwise AND** takes two equal-length binary representations and performs the logical AND operation on each pair of the corresponding bits, which is equivalent to multiplying them. Thus, if both bits in the compared position are 1, the bit in the resulting binary representation is 1 (1 × 1 = 1); otherwise, the result is 0 (1 × 0 = 0 and 0 × 0 = 0). For example:

```
    0101 (decimal 5)
AND 0011 (decimal 3)
  = 0001 (decimal 1)
```

#### 按位取反~

The **bitwise not** (`~`, also called the ones complement operator) is a unary operator – it only takes one argument (all other bitwise operators are binary operators). Bitwise not produces the opposite of the input bit – a one if the input bit is zero, a zero if the input bit is one.  For example:

```
NOT 0111  (decimal 7)
  = 1000  (decimal 8)
```

```
NOT 10101011  (decimal 171)
  = 01010100  (decimal 84)
```

#### 按位或|

The **bitwise or** operator `|` produces a one in the output bit if either input bit is a one and produces a zero only if both input bits are zero.  For example:

```
   0101 (decimal 5)
OR 0011 (decimal 3)
 = 0111 (decimal 7)
```

#### 异或^

The **bitwise exclusive or**, or xor `^` produces a one in the output bit if one or the other input bit is a one, but not both. For example:

```
    0101 (decimal 5)
XOR 0011 (decimal 3)
  = 0110 (decimal 6)
```

Bitwise operators can be combined with the `=` sign to unite the operation and assignment: `&=`, `|=`, and `^=` are all legitimate operations (since `~` is a unary operator it cannot be combined with the `=` sign).  

对一个变量用同一个值异或两次，等于什么也没做

X^Y^Y -> X

```c++
#include <iostream>
#include <cstring>
using namespace std;

int main (){

	char str[] = "Hello World!";
	string cpy = "";
	int i = 0;
	for(;i < strlen(str); i++){
		cpy += str[i]^3;
	}
	cout << cpy <<endl;
	for(i = 0;i < cpy.size(); i++){
		cpy[i] ^= 3;
	}
	cout << cpy <<endl;
    return 0;
}
```

![](/assets/img/posts/questions/Snipaste_2018-09-29_20-20-08.png)

### 移位运算(Shift operators )

The shift operators also manipulate bits. The left-shift operator `<<` produces the operand to the left of the operator shifted to the left by the number of bits specified after the operator. The right-shift operator `>>` produces the operand to the left of the operator shifted to the right by the number of bits specified after the operator. **If the value after the shift operator is greater than the number of bits in the left-hand operand, the result is undefined. If the left-hand operand is unsigned, the right shift is a logical shift so the upper bits will be filled with zeros. If the left-hand operand is signed, the right shift may or may not be a logical shift (that is, the behavior is undefined).**  

Shifts can be combined with the equal sign (`<<=` and `>>=`). The left value is replaced by the left value shifted by the right value.  

```c
#include <stdio.h>
int main(void){
	unsigned char c = 0xA5;
	printf(" c=%hhx\n",c);
	printf("c<<2=%x\n",c<<2);
	printf(" c=%d\n",c);
	printf("c<<2=%d\n",c<<2);
	return 0;
}
```

![](/assets/img/posts/questions/Snipaste_2018-09-29_21-18-34.png)

如果我们将一个数向左移n为，等价于它乘2的n次方。

![](/assets/img/posts/questions/Snipaste_2018-09-29_21-44-40.png)

```c
#include <stdio.h>
int main(){
	int a = 0x80000000;
	unsigned int b = 0x80000000;
	printf("a=%d\n", a);
	printf("b=%u\n", b);
	printf("a>>1=%d\n", a>>1);
	printf("b>>1=%u\n", b>>1);
	printf("a<<1=%d\n", a<<1);
	printf("b<<1=%u\n", b<<1);
	return 0;
}
```

![](/assets/img/posts/questions/Snipaste_2018-09-29_22-11-05.png)

对于有符号整形，正数右移左边补一，左移右边补零。负数左移右移都补零。

```c
#include <stdio.h>
int main(){
	int a = 0x80000000;
	int b = 0x40000000;
	int c = 0x00000001;
	int d = 0x00000000;
	int e = 0x80000001;
	int f = 0x80000000;
	printf("    a=%d\n", a);
	printf("    a=%x\n", a);
	printf("a>>21=%x\n", a>>21);
	printf("a>>21=%d\n", a>>21);
	printf("    b=%d\n", b);
	printf("    b=%x\n", b);
	printf("b>>21=%x\n", b>>21);
	printf("b>>21=%d\n", b>>21);
	printf("    c=%d\n", c);
	printf("    c=%x\n", c);
	printf("c<<11=%x\n", c<<11);
	printf("c<<11=%d\n", c<<11);
	printf("    d=%d\n", d);
	printf("    d=%x\n", d);
	printf("d<<11=%x\n", d<<11);
	printf("d<<11=%d\n", d<<11);
	printf("    e=%d\n", e);
	printf("    e=%x\n", e);
	printf("e<<11=%x\n", e<<11);
	printf("e<<11=%d\n", e<<11);
	printf("    f=%d\n", f);
	printf("    f=%x\n", f);
	printf("f<<11=%x\n", f<<11);
	printf("f<<11=%d\n", f<<11);
	return 0;
}
```

![](/assets/img/posts/questions/Snipaste_2018-09-30_21-51-11.png)
