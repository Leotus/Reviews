# L1
# Analysis of Algorithms
performance and resource usage
# Problem: Sorting
```
Input:sequance<a1,a2,...an> of numbers
Output:permutation<a1',a2',...an'>,and a1'< a2'<...an'
```
## Insertion sort(插入排序)
```
Insertion sort A[1...n]
for j = 2 to n
    do key = A[j]
        i = j-1
        while i>0 and A[i]>key
            do A[i+1] = A[i]
                i = i -1
        A[i+1]=key
```
- Running time
    1. Depends of input(eg.already sorted)
    2. Depends of input size
    3. Want upper bounds(guarantee of users)
- Kinds of analysis
    1. Worst-case:(usually)
        - T(n):max time on any input of size size n
    2. Average-case:(sometimes)
        - T(n):expected time over all inputs of size n(Need assumption of stat.distr.)
    3. Best-case(bogus,骗人的)
        - Cheat
- What's the ins sort's worst time?
    - Depends on computer
        - relative speed(on same computer)
        - absolute speed(on diff computer)
- BIG IDEA!!!:Asymptotic analysis
    1. Ignore machine dependant constants
    2. Look at growth of T(n) as n->无穷

### Asymptotic notation(渐进符号)
- θ-notation: Drop low order tems. Ingore leading constants
```
Ex : 3n^3 + 90n^2 - 5n + 6046 = θ(n^3)
```
- As n->infinity ,θ(n^2)alg always beats θ(n^3) alg(We look at the growth of T(n))

### Insertion sort analysis
Worst case:Input reverse sorted
- T(n) = j 从 2 to n求和（θ(j)） = θ(n^2)

## Merge sort(归并排序)
1. If n=1,done
2. Recursively sort A[1...n/2] and A[n/2+1...n]
3. merge two sorted lists

```
Key subroutine :Merge
    l1: 2 7 13 20
    l2:1 9 11 12
every time: find the min(on the front of every list) and write
    1 2 7 9 11 13 20
Time = θ(n) on n total elem s
```
- Recurrence 
    - T(n) = θ(1) if n = 1(usually omit,通常省略这个)
    - T(n) = 2T(n/2) + θ(n) = θ(nlgn) if n>1


# L2

# Asymptotic notation(渐进符号)
# Big-O
- f(n) = O(g(n)) (等号不是等于的意思，是上界)( means is not equal  )
```
means there are consts C>0 and n0>0 such that 0<= f(n)<=Cg(n) for all n>=n0 
Ex: 2n^2 = O(n^3)

根据上面可以做出定义：
Set definition:
O(g(n)) = { f(n) | there are constants c>0,n0>0 such that 0<=f(n)<=Cg(n) for all n>=n0 }
```
- Means Convention
    - A set in a formula represents an anousymous function in that set.
```
Ex: f(n) = n^3 + O(n^2)
means there is a function h(n) < O(n^2) such tath 
f(n) = n^3 + h(n) 


Ex: n^2 + O(n) = O(n^2)
means for any f(n) ∈ O(n) there is an h(n) ∈ O(n^2) such that n^2+f(n) = h(n)
```
# Big-Ω：
```
Ω(g(n)) = { f(n) | there are exsit consts C>0, n0>0,such that 0<=Cg(n)<=f(n) for all n>=n0 }

Ex:root n = Ω(lgn)
```
- Analogies(类比)
```
Big-O Big-Ω     θ   little-o    little-omega
<=      >=      =       <              >
```
# θ notation
- θ(g(n)) = O(g(n)) ∩ Ω(g(n)) (交集)

# little o & ω notations
```
Ex 2n^2 = o(n^3)
```

# Solving recurrences
# Substitution method
    1. Giess the form of the solution
    2. verify by induction
    3. Solve for consts

```
Ex: T(n) = 4T(n/2) + n ; [T(1) = θ(1)]
- Guess T(n) = O(n^3)
- Assume T(k) <= Ck^3 for k < n
- T(n) = 4T(n/2)+n
        <= 4 C (n/2)^3 + n = 1/2 C n^3 + n
                           = Cn^3 - (1/2 C n^3 + n)
        <= Cn^3 if (1/2 C n^3 + n) >= 0 eg C>=1,n>1
- Base T(1) = θ(1) <= C 1^3 if C chosen suffi large

- Try T(n) = O(n^2)
- Assume T(k) <= Ck^2 for k < n
- T(n) = 4T(n/2)+n
       <= 4 C (n/2)^2 + n = C n^2 + n
                          = C n^2 - (-n)
       <= C n^2 if (-n) >= 0 不成立

如果你有神的指示，你能写出下面的假设
- Assume T(k) <= C1 k^2 - C2 k for k<n
- T(n) = 4T(n/2)+n
        <=  4( C1 (n/2)^2 - C2(n/2) )+ n
        = C1n^2 + (1-2C2)n
        = C1 n^2 - C2 n - (-1+C2)n
        <= C1 n^2 - C2 n if (C2 - 1) >= 0 if C2 >= 1
- Base T(1) <= C1 - C2 if C1 suff large 
```

# Recursion-tree method
```
Ex T(n) = T(n/2) + T(n/4) + n^2
```
Expand 基本展开写法。代表加起来
```
T(n) =    n^2
       |       |
    T(n/2)      T(n/4)
```
```
T(n) =    n^2                =      n^2
       |       |                |       |
    T(n/2)      T(n/4)      (n/2)^2     (n/4)^2
                        |       |       |       |
                    T(n/4)  T(n/8)      T(n/8) T(n/16)

                    ...
                    =   n^2          ———————————————n^2
                     |       |
                (n/2)^2     (n/4)^2 ——————————————5/16n^2
            |       |       |       |
         (n/4)^2  (n/8)^2   (n/8)^2  (n/16)^2  ——25/256n^2
                        ...
        θ(1)  θ(1)  θ(1)  θ(1)  .....


        叶子节点总数 <=  n
        每层分别求和
        n^2
        5/16n^2
        25/256n^2
        发现是一个等比数列

knows:
1 + 1/2 + 1/4 + .... = 2

Total (level-by-level):
 <= (1+5/16+25/256+....)n^2
 < 2 n^2
 = O(n^2)
```

# Main Method
applies to recurrences of the form ```T(n)=a(n/b)+f(n)``` where a>=1,b>=1,f(n) asymptotically positive(渐进趋正)，存在一个n>=n0,f(n)>0
```
compare f(n) with n^logb(a)
case1   f(n) = O(n^(logb(a) - ε))
                for some ε > 0
        => T(n)= θ(n^logb(a))
case2   f(n) = θ(n^(logb(a)) * (lgn)^k)
                    for some k >= 0
        => T(n)= θ(n^logb(a) * (lgn)^(k+1))
case3   f(n) = Ω(n^(logb(a) + ε))
                    for some ε > 0
                & af(n/b) <= (1-ε')f(n)
                    for some ε' > 0
        => T(n)= θ(f(n))
```

```
Ex T(n) = 4T(n/2) + n

    a = 4,b = 2,f(n) = n
    n^logb(a) = n ^ log2(4) = n^2 >= f(n) = n
    Case1 => T(n) = θ(n^2)
```
```
Ex T(n) = 4T(n/2) + n^2

    a = 4,b = 2,f(n) = n^2
    n^logb(a) = n ^ log2(4) = n^2 = f(n) = n^2
    Case2 => T(n) = θ(n^2 * lgn)
```
```
Ex T(n) = 4T(n/2) + n^3

    a = 4,b = 2,f(n) = n^3
    n^logb(a) = n ^ log2(4) = n^2 < f(n) = n^3
    Case3 => T(n) = θ(n^3)
```
```
Ex T(n) = 4T(n/2) + n^2/lgn

    a = 4,b = 2,f(n) = n^2/lgn
Main Method did not apply
```
---------------------------------------------------------
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

# 树1

### 静态查找1：顺序查找(\*)
```
int SequentialSearch(List Tb1,ElementType K)
{/*在Element[1]~Element[n]中查找关键字为K的数据元素*/
    int i;
    Tb1->Element[0] = K;//建立哨兵
    for(i = Tb1->Length;Tb1->Element[i] != K;i--);
    return i;/*查找成功返回所在单元下标；不成功返回0*/
}
typedef struct LNode *List;
struct LNode{
    ElementType Element[MAXSIZE];
    int Length;
};
```

### 静态查找2：二分查找(\*)
先决条件：放在数组里的有序的数列
```
int BinarySearch(List Tb1,ElementType K)
{
    /*在表Tb1中查找关键字为K的数据元素*/
    int left,right,mid,NoFound=-1;

    left = 1;   /*初始左边界*/
    right = Tb1->Length; /*初始右边界*/
    while(left <= right)
    {
        mid = (right + left)/2; /*计算中间元素坐标*/
        if(K < Tb1->Element[mid]) right = mid - 1; /*调整右边界*/
        else if(K > Tb1->Element[mid])  left = mid + 1; /*调整左边界*/
        else return mid; /*查找成功，返回数据元素的下标*/
    }
    return NotFound; /*查找不成功，返回-1*/
}
```
### 树与非树
- 子树是不相交的
- 除了根结点外，每个结点有且仅有一个父节点
- 一颗N个结点的树有N-1条边（N个结点只有根结点没有与父节点链接的边）

### 术语
- 结点的度：结点的子树个数
- 树的度：树的所有结点中最大的度数
- 结点的层次：规定根结点在1层，其他任一结点的层数是其父结点层数+1
- 树的深度：树中所有结点中的最大层次是这颗树的深度

### 树的表示
通常用链表表示，为了不产生过多的空间浪费，引入如下的表示法
- 儿子-兄弟表示法(二叉树)
```
---------------------------
|   Element               |
---------------------------
|FirstChild | NextSibling-|->
-------|-------------------
       |
       V
```

### 二叉树的定义及性质
- 完美/满二叉树
- 完全二叉树：有n个结点的二叉树，对树中结点按从上至下、从左至右顺序进行编号，编号为i（1<=i<=n）结点与满二叉树中编号为i结点在二叉树中的位置相同（其余层都是满的，最后一层只允许从右往左依次缺失）
- 一个二叉树第i层的最大结点数为2^(i-1),i>=1
- 深度为k的二叉树有最大结点数为(2^k)-1,k>=1(满二叉树)
- 对任何非空二叉树T，若n0表示叶结点的个数、n1是度为1的非叶结点个数、n2是度为2的非叶结点个数，那么两者满足n0=n2+1；```（由n0+n1+n2-1=0*n0+1*n1+2*n2）这个边个数等式得出```

### 二叉树遍历形式
- PreOrderTraversal(BinTree BT):先序，根、左、右
- InOrderTraversal(BinTree BT)：中序，左、根、右
- PostOrderTraversal(BinTree BT)：后序，左、右、根
- LevelOrderTraversal(BinTree BT)：层次，上到下，左到右

### 二叉树存储结构
1. 顺序存储结构
    - 完全二叉树：按从上到下，从左到右顺序存储n个结点的完全二叉树的结点父子关系-
        - 非根结点（i>1）的父结点的序号是Li/2J
        - 结点（序号为i）的左孩子结点的序号是2i，若2i<=n,否则没有左孩子
        - 结点（序号为i）的右孩子结点序号是2i+1，若2i+1<=n,否则没有右孩子
    - 一般二叉树可以补齐成完全二叉树存储，会浪费空间
2. 链表存储
```
typedef struct TreeNode *BinTree;
typedef BinTree Position:
struct TreeNode{
    ElementType Data;
    BinTree Left;
    BinTree Right;
}
```
### 二叉树的遍历(\*)
1. 先序遍历
```
void PreOrderTraversal(BinTree BT)
{
    if(BT){
        printf("%d",BT->Data);
        PreOrderTraversal(BT->Left);
        PreOrderTraversal(BT->Right);
    }
}
```
2. 中序遍历
```
void InOrderTraversal(BinTree BT)
{
    if(BT){
        InOrderTraversal(BT->Left);
        printf("%d",BT->Data);
        InOrderTraversal(BT->Right);
    }
}
```
2. 后序遍历
```
void PostOrderTraversal(BinTree BT)
{
    if(BT){
        PostOrderTraversal(BT->Left);
        PostOrderTraversal(BT->Right);
        printf("%d",BT->Data);
    }
}
```
### 二叉树的非递归遍历(\*)
以中序遍历为例子
```
void InOrderTraversal(BinTree BT)
{
    BinTree T = BT;
    Stack S = CreateStack(MaxSize);/*创建并初始化堆栈*/
    while(T || !isEmpty(S)){
        while(T){/*一直向左并将沿途结点压入堆栈*/
            // printf("%5d",T->Data);/*放这里就是先序*/
            Push(S,T);
            T = T->Left;
        }
        if(!IsEmpty(S)){
            T = Pop(S);/*结点弹出堆栈*/
            printf("%5d",T->Data);/*(访问)打印结点*/
            T = T->Right;/*转向右子树*/
        }
    }
}
```

### 层次遍历(\*)
```
void LevelOrderTraversal(BinTree BT){
    Queue Q;    BinTree T;
    if(!BT)return;//空树直接返回
    Q = CreateQueue(MaxSize);//创建并初始化队列Q
    AddQ(Q,BT);
    while(!IsEmptyQ(Q)){
        T = DeleteQ(Q);
        printf("%d\n",T->Data);// 访问取出队列的结点
        if(T->Left) AddQ(Q,T->Left);
        if(T->Right) AddQ(Q,T->Right);
    }
}
```

### 应用1：输出二叉树中的叶子结点(\*)
```
void PreOrderTraversal(BinTree BT)
{
    if(BT){
        if(!BT->Left && !BT->Right)
            printf("%d",BT->Data);
        PreOrderTraversal(BT->Left);
        PreOrderTraversal(BT->Right);
    }
}
```
### 应用2：求二叉树的高度(\*)
```
void PostOrderGetHeight(BinTree BT)
{
    int HL,HR,MaxH;
    if(BT){
        HL = PostOrderGetHeight(BT->Left); // 左子树的高度
        HR = PostOrderGetHeight(BT->Right); //右子树的高度
        MaxH = (HL > HR) ? HL : HR; //左右最大子树的高度
        return (MaxH + 1); // 返回树的深度
    }
    else return 0;//树高度为0
}
```
### 应用3：由树得到运算表达式
answer：中序遍历要得到中缀表达式，必须在左子树进栈前加左括号，出栈后加右括号
### 应用4：由两种遍历序列确定二叉树
answer：必须要有中序遍历才行

### 应用5：树的同构(\*)(\*)
- 给定两颗树T1和T2，如果T1可以通过若干次左右孩子互换就变成T2，则我们称两棵树是同构的，现在给定两棵树，请你判断是不是同构的。
```
输入样例
8
A 1 2
B 3 4
C 5 -
E 6 -
G 7 -
F - -
H - -
8
G - 4
B 7 6
F - -
A 5 1
H - -
C 0 -
D - -
E 2 -
```
- 求解思路
    1. 二叉树表示（这次使用结构数组表示二叉树：静态链表）
```
     ------------------
     |A  |B|C |D |   |
     ------------------
Left |-1 |2|-1|-1|   |
     ------------------
Right |1  |3|-1|-1|   |
     -----------------
       0    1  2  3  4
```
```
#define MaxTree 10
#define ElementType char
#define Tree int
#define Null -1

struct TreeNode
{
    ElementType Element;
    Tree Left;
    Tree Right;
}T1[MaxTree],T2[MaxTree];
```
    2. 建立二叉树
```
Tree BuildTree(struct TreeNode T[])
{
    scanf("%d\n",&N);
    if(N){
        for(i=0;i<N;i++) check[i]=0;
        for(i=0;i<N;i++){
            scanf("%c %c %c\n",%T[i].Element,&cl,&cr);
            if(cl!='-'){
                T[i].Left = cl - '0';
                check[T[i].Left] = 1;
            }
            else T[i].Left = Null;
            if(cr!='-'){
                T[i].Right = cr - '0';
                check[T[i].Right] = 1;
            }
            else T[i].Right = Null;
        }
        for(i=0;i<N;i++)
            if(!check[i])break;// 以check的方式，找出根结点
        Root = i;
    }
    return Root;
}
```
    3. 同构判别
```
int Isomorphic(Tree R1,Tree R2)
{
    if((R1==Null)&&(R2==Null)) return 1;// 左右都是空的
    if( ((R1==Null)&&(R2!=Null)) || ((R1!=Null)&&(R2==Null)) )
        return 0;//只有一边是空的，那肯定不行
    if( T1[R1].Element != T2[R2].Element ) return 0;// 根结点都不同，肯定么得
    if( ( T1[R1].Left == Null )&&( T2[R2].Left == Null ))
        // 两个树左边都是空的，只要判断右边
        return Isomorphic(T1[R1].Right,T2[R2].Right);
    if( ((T1[R1].Left!=Null)&&(T2[R2].Left!=Null))&&
        ((T1[T1[R1].Left].Element)==(T2[T2[R2].Left].Element)) )
        // 两个树左边都是非空，而且左子结点相等，此时无需交换左右，只要左子树和右子树即可
        return (Isomorphic(T1[R1].Left,T2[R2].Left) &&      Isomorphic(T1[R1].Right,T2[R2].Right));
    else // 需要交换左右子树判断
        return (Isomorphic(T1[R1].Left,T2[R2].Right) &&      Isomorphic(T1[R1].Right,T2[R2].Left));
}
```
```
int main()
{
    Tree R1,R2;

    R1 = BuildTree(T1);
    R2 = BuildTree(T2);
    if(Isomorphic(R1,R2)) printf("Yes\n");
    else printf("No\n");

    return 0;
}
```
# 树 2

### 二叉搜索树(BST)/二叉排序树/二叉查找树
- 二叉搜索树：
    1. 非空左子树的所有键值小于其根节点的键值
    2. 非空右子树的所有键值大于其根节点的键值
    3. 左、右子树都是二叉搜索树

#### 查找
```
递归查找：
Position Find(ElementType X,BinTree BST)
{
    if(!BST) return NULL;// 查找失败
    if(X > BST->Data) return Find(X,BST->Right); /* 在右子树中继续查找 */
    else if(X < BST->Data) return Find(X,BST->Left); /* 在左子树中继续查找 */
    else return BST;/* 找到了 */
}
非递归查找：
Position IterFind(ElementType X,BinTree BST)
{
    while( BST ){
        if(X > BST -> Data)
            BST = BST -> Right;
        else if(X < BST -> Data)
            BST = BST -> Left;
        else
            return BST;
    }
    return NULL;
}
```
```
最小查找递归实现：
Position FindMin(BinTree BST)
{
    if(!BST) return NULL;
    else if(!BST->Left)
        return BST;
    else
        return FindMin(BST->Left);
}
最大查找迭代实现：
Position FindMax(BinTree BST)
{
    if(BST)
        while(BST->Right) BST = BST->Right;
    return BST;
}

```
#### 插入
```
BinTree Insert(ElementType X,BinTree BST)
{
    if(!BST){
        // 若树为空，生成并返回一个结点的二叉树
        BST = malloc(sizeof(struct TreeNode));
        BST->Data = X;
        BST->Left = BST->Right( = NULL;
    }else // 开始找要插入元素的位置
        if(X<BST->Data)
            BST->Left = Insert(X,BST->Right);
                // 递归插入左子树
        else if(X>BST->Data)
            BST->Right = Insert(X,BST->Right);
                // 递归插入右子树
        // else X已经存在，什么都不做
    return BST;
}
```
#### 删除
1. 删除的结点没有儿子，直接删除
2. 删除的结点只有一个儿子，儿子代替原来的位置
3. 删除的结点有两个儿子，在左子树中找一个最大的，或者在右子树中找一个最小的代替原来的位置

```
BinTree Delete(ElementType X,BinTree BST)
{
    Position Tmp;
    if(!BST) cout << "没找到";
    else if(X < BST->Data)
        BST->Left = Delete(X,BST->Left);//左子树递归删除
    else if(X > BST->Data)
        BST->Right = Delete(X,BST->Right);//右子树递归删除
    else 
        if(BST->Left && BST->Right){// 被删除的结点有左右两个子树
            Tmp = FindMin(BST->Right);
            BST->Data = Tmp->Data;
            BST->Right = Delete(BST->Data,BST->Right);
        } else{
            Tmp = BST;
            if(!BST->Left){//有右孩子或无子结点
                BST = BST->Right;
            }
            else if(!BST->Right)
                BST = BST->Left;//有左孩子或无子结点
            free(tmp);
        }
    return BST;
}
```
### 平衡二叉树

# 图 1

# 图 2

# 图 3

# 排序 1

# 排序 2

# 散列查找

# 综合
