# 绪论

### 1.6.1 信息存储单元
```
位（bit，b）
字节（byte，B）——1byte = 8 bit
千字节 1KB = 1024B
兆字节 1MB = 1024K
...
```

### 1.6.2 数字系统
```
 3个二进制位 = 1个八进制位
 4个二进制位 = 1个十六进制位

 R进制->十进制： 各位数字与权相乘，积相加
    (1111.11)2 = 1*2^3 + 1*2^2 + 1*2^1 + 1*2^0 + 1*2^-1 + 1*2^-2 = (15.75)10

 十进制整数->R进制： 除R取余数，反着写
    2|___15_____
        2|_____7_____ ------- 1
            2|____3_____ --------1
                2|____1_____ --------1
                    2|____0_____ --------1
 十进制小数->R进制： 乘R取整数，正着写
    0.75 * 2 = 1.50 ------ 1
    0.5 * 2 = 1.0 --------1
```

### 1.6.3 数据的编码表示
#### 整数
```
原码：(加一个符号位，+ => 0、- => 1，数值位用绝对值表示)，0有两个，计算的时候符号位单独处理

反码：
    1、正整数是本身
    2、负整数，符号位不变，其他按位取反

补码：（0表示唯一，符号位参与运算，计算结果仍然是补码）
    1、正数补码 = 原码
    2、负数补码 = 反码 + 1
```
#### 浮点数
```
    N = M * 2^E
    E: 阶码，反映数据表示范围
    M：尾数，反映数据精度
```
#### 字符
```
    编码、ASCII...
```

# CPP简单程序设计

### 2.3.1 基本数据类型、常量、变量

#### 常量
```
整数常量：
    十进制：0~9组成的数字，开头不为0
    八进制：开头为0的0~7的组合数字
    十六进制：0x开头的0~9和A~F（a~f）的组合
    后缀：L(l)long型、LL(ll)long long型、U(u)unsigned型

浮点常量：！！！在计算机里都是近似存储的，不要试图比较两个浮点数完全相等
    一般：12.5
    指数：0.345E+2、-34.4E-3
    后缀：浮点型默认位double型，如果后缀F(f)就是float型，12.3f

C风格字符串常量：
    按位存放，末尾'\0'，用”“表示

```
#### 变量
```
    初始化：
        int a = 0
        int a(0)
        int a = {0}
        int a{0}

    !!! {}初始化列表的形式进行初始化，不允许发生精度损失的数据丢失
```

#### 符号常量
```
    const 数据类型 常量名 = 常量值
或  数据类型 const 常量名 = 常量值

    定义后必须初始化，之后不能修改（只读）
```

### 2.4.2 逗号运算
```
    先求表达式1，再求表达式2，结果是后面的表达式的值
    a = 3*5,a*4 结果是60
```

### 2.4.3 sizeof、位运算
```
sizeof:
    sizeof(类型名) 或者 sizeof 表达式
    结果 = 类型或者表达式的结果所占的 字节数
位运算：
    按位与：&
    按位或：|
    按位异或：^
    按位取反：~
    移位：<<(高位舍弃、低位补0)、>>（低位舍弃，无符号数高位补0，有符号数高位补符号位）
```

### 2.4.4 运算优先级、类型转换
```
隐式类型转换：低->高
显式类型转换：
    C风格：（不建议）
        类型说明符（表达式）
        （类型说明符）表达式
    C++风格：（强烈建议）
        类型转换操作符<类型说明符>(表达式)

        类型转换操作符:
        const_cast、dynamic_cast、reinterpret_cast、static_cast
```

### 2.5.1 数据的输入和输出
```
常用的I/O流类库控制符
    dec 十进制表示
    hex 十六进制表示
    oct 八进制表示
    ws 提取空白符
    endl 插入空行符，并刷新流
    ends 插入空字符
    setsprecision(int) 设置浮点数的小数位数（包括小数点）
    setw(int) 设置域宽

例：
    cout << setw(5) << setsprecision(3) << 3.1415;
```

### 2.5.2 自定义类型
```
别名：
    typedef 已有类型名 新类型名
    using 新类型名 = 已有类型名

枚举类型：
 C风格：
    定义方式 将全部可取值一一列举出来
    语法形式
        enum 枚举类型名 {变量值列表}
    例：enum Weekday {SUN,MON,TUE,WED,THU,FRI,SAT},默认SUN=0,MON=1...
    枚举在声明时另指定元素的值
        如：enum Weekday {SUN=7,MON=1,TUE,WED,THU,FRI,SAT}，默认TUE=2,WED=3...
    整数不能给枚举类型赋值，要进行类型转换，反过来可以
```
```
#include <iostream>
using namespace std;
enum GameResult { WIN, LOSE, TIE, CANCEL};
int main(){
    GameResult result;//可以不写enum
    enum GameResult omit = CANCEL;
    for(int count = WIN ; count <= CANCEL ; count++){
        result = GameResult(count);//整数不能直接给枚举类型赋值
        if(result == omit)
            cout << "cancelled" << endl;
        else{
            cout << "played";
            if(result == WIN) cout <<" and won" << endl;
            if(result == LOSE) cout <<" and lost" << endl;
        }
    }
    return 0;
}

```

```
auto 与 decltype类型
    auto:通过初始化自动推断类型
    decltype：定义一个变量与某一个表达式的类型相同，但并不用该表达式的初始化变量
        decltype(i) j = 2
            表示j以2为初始值，类型与i一致
```

# 函数

```
精度的处理:
例：不用库函数求sin函数，精度为10^-10


const double TINY_VALUE = 1e-10; // 计算精度为10^-10
double tsin(double x){
    double g = 0;
    double t = x;
    int n = 1;
    do{
        g += 1;
        n++;
        t = -t * x * x/(2*n - 1)/(2*n - 2); // 泰勒展开
    }while(fabs(t) >= TINY_VALUE) // 就是最后加的一项，要大于精度值
    return g;
}

```
```
// 产生一个1~6的随机数
cin >> seed; // 种子
srand(seed); // 将种子传递给rand
int i = 1 + rand() % 6;
```
### CONSTEXPR函数
constexpr修饰的函数在其所有参数都是constexpr时一定返回constexptr
```
constexpr int get_size(){return 20;}

constexpr int foo = get_size();//正确：foo是一个常量表达式
```

### 函数重载
1) 形参类型不同
2) 形参个数不同
3) **返回值类型不同不能算函数重载

# 面向对象

### 面向对象程序的基本特点
1) 抽象
2) 封装
3) 继承
4) 多态

### 委托构造函数
委托构造函数使用类的其他构造函数执行初始化过程
例：
```
Clock(int newH,int newM,int newS):hour(newH),minute(newM),second(newS){}
Clock:Clock(0,0,0){}
```

### 复制(拷贝)构造函数
发生：
1) 用一个对象初始化另一个
2) 用实参初始化形参
3) 返回一个临时的对象
```
int fun1(Point d){
    return 0;
}

Point fun2(){
    Point c;
    return c;
}

Point b(a); // （1）
fun1(b); // （2）
b = fun2(); // （3） 这里其实还进行了赋值运算
```

### 前向引用声明
例：
```
class B; // 前向引用声明
class A{
public:
    void f(B b);
};
class B{
public:
    void g(A a);
}
```

### 结构体
```
struct Student{
    int num;
    string name;
    char sex;
    int age;
}
Student stu = {97000,"Lin",'F',19}; // 结构体的初始化方式
```

### 联合体
成员共用同一组内存单元
任何两个成员不会同时有效
```
union 联合体名称{
    公有成员
protected:
    保护型成员
private:
    私有成员
}
```
例：
```
union Mark{ //表示成绩联合体
    char grade; //等级制的成绩
    bool pass; //只记录是否通过的成绩
    int percent; //百分制的成绩
};
```
说明变量grade、pass、percent将公用内存空间，后面如果改变一个，其他的也会改变
```
union{
    int i;
    float j;
}
i = 1;
j = 2.5; //最后都会变成2.5，因为公用内存空间
```

### 枚举类
```
enum class 枚举类型名:底层类型{枚举值列表};//不指定底层类型，就是int
```
枚举类与C风格枚举相比好处：
1) 强作用域，使用的时候要加上类型名，如：Type::General
2) 转换限制，枚举类对象不再可以与整型进行转换
3) 可以指定底层类型

# 数据的共享与保护