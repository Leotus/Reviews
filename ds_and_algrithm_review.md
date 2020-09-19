# 基本概念

### 时钟打点

```
#include <time.h>
clock_t start,stop;
double duration;
int main(){
    start = clock();// 测试开始
    XXXXX
    stop = clock(); // 测试结束
    duration = ((double)(stop - start))/CLK_TCK;

    return 0;
}

```

### 最大子列和问题（\*）

```
n^3: i循环头，j循环尾，k循环中
n^2: i循环头，j循环尾，每一次在前面的基础上加一
nlogn：中间分一半，递归，每次取max{左边最大子列和，右边最大子列和，跨越中间最大子列和}
n：在线处理，每一个数，如果当前子列和小于0，直接抛弃，如果大于maxSum，更新maxSum，最后就是最终解
```

# 线性结构

## 栈

top 进 top 出

### 顺序栈

```
typedef struct{
    ElemType *elem;//数组
    int top;
    int size;
    int increment;
} SqStack;
```

### 链式栈

用链表表示栈的时候，他的 top 一定在链表的头上

```
typedef struct{
    ElemType data;
    struct LNode* Next;
} LNode,*LinkStack;// 带头节点
```

## 栈

rear 进 front 出

### 顺序队列

```
typedef struct{
    ElemType *elem;//数组
    int front;
    int rear;
    int maxSize;
} SqQueue;
```

1. 不循环队列入队：SqQueue.rear++;
2. 循环队列的情况下:n 个空间最多放 n-1 个数，
   ```
   空的条件是SqQueue.rear == SqQueue.front,
   满的条件是(SqQueue.rear+1)%SqQueue.maxSize == SqQueue.front.
   入队：SqQueue.rear = (SqQueue.rear+1)%SqQueue.maxSize;
   ```

### 链式队列

用链表表示队列的时候，他的 front 在表头，rear 在表尾

```
typedef struct{
    ElemType data;
    struct LNode* Next;
} LNode;
typedef struct{
    LNode* rear;
    LNode* front;
} *LinkQueue;
```

### 单链表的逆转(\*)

```
// 三个指针,带头节点
Ptr Reverse(Ptr head,int K)
{
    cnt = 1;
    new = head->next;
    old = new->next;
    while(cnt < K>){
        tmp = old->next;
        old->next = new;
        new = old;
        old = tmp;
        cnt++;
    }
    head->next->next = old;
    return new;
}
```

oj 取巧:用顺序表存储,先排序,再直接逆序输出

测试数据:

- 有尾巴不需要反转的情况
- 地址取到上下界
- 正好全反转
- K = N 全反转
- K = 1 不用反转
- 最大(最后剩 K-1 不反转)、最小 N
- 有多余结点(让你取巧的挂掉)

# 树 1

# 树 2

# 树 3

# 图 1

# 图 2

# 图 3

# 排序 1

# 排序 2

# 散列查找

# 综合
