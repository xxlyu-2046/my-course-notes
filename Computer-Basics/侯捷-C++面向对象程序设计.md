## Summary

上

- 例子1——不含指针的复数类：介绍C++头文件、类、构造函数、参数传递与操作符重载。
- 例子2——含指针的string类：介绍析构函数（释放动态内存）、拷贝构造/赋值（深拷贝）、new（先分配memory再调用ctor）/delete（先调用dtor再释放memory）；array new一定要搭配array delete。
- 类之间的关系：继承、复合、委托，其ctor和dtor的顺序关系，以及三种关系的组合。

下
- 类型转换与explicit使用
- 模板与STL、C++11新特性
# C++程序设计（上）

## 不含指针的例子：复数类

complex.h 、 complex-test.cpp

**Object Based(基于对象) vs Object Oriented(面向对象)** 

- Object Based:面向的是单一class的设计 

- Object Oriented:面对的是多重classes的设计，classes和classes之间的关系。 

### 头文件与类的声明

**C++ programs代码基本形式** 

![img](./assets/93f15e0c30c9ec51129ec4571b2fa282-150143.png)

**Header(头文件)中的防卫式声明** 

ifndef+define。（这样如果程序是第一次引用它，则定义，后续因为已经定义了，所以不会重复引用）。<u>你写的任何一个头文件都需要加这个防卫式声明。</u>

![img](./assets/b3c2a349a1a9114fcd40cd42ac978cfd-226541.png)

 **Header(头文件)的布局** 

- 前置声明
- 类声明
- 类定义

![img](./assets/73cf18454f7577d1e03cf7cf145160d5-203536.png)

**class的声明(declaration)** 

![img](./assets/53a8d2f36c5b4c6edd9578475ffaa3e5-216100.png)

**inline(内联)函数** 

![img](./assets/c196932d551e4e391e93ed62113fad43-199766.png)

### constructor(ctor,构造函数) 

![img](./assets/86dcbf7dc6adec2f5c901c8ef3dd9bd7-281894.png)

**ctor (**構造函數**)** 可以有很多個 **– overloading (**重載**)** 

黄色标注的构造函数定义将出现问题，如果该函数与上面构造函数同时出现，在无参初始化该类对象时将产生冲突，因为第一个构造函数已经有参数默认初始化列表了，定义该类对象时可以不加入参数，这就产生了冲突。

![img](./assets/2c85d94ea44b66a810186d5d1ed1f80b-128970.png)

**constructor (ctor,** 構造函數**)** 被放在 **private** 區 

单例模式。

1. 私有化它的构造函数，以防止外界创建单例类的对象；
2. 使用类的静态变量指向类的唯一实例；
3. 使用一个公有的静态成员函数获取该实例。

> 由于构造函数是私有的，class外部无法实例化对象。但class内部的静态成员函数可以，且静态函数可以被外部访问（通过类，不需要实例化对象）。

![img](./assets/1c5676cf45c1198ad8f6b95f2d64100b-71520.png)

**const member functions (**常量 成員函數**)** 

在一个类中，如果成员函数中没有改变成员函数操作（例如get操作），那么建议在该方法声明处加入const关键字，如果不加入const关键字，那么c++编译器将认为该函数可能会修改类的成员变量。这样做有个好处，可以看到上图的"？！"一图，使用者利用了const关键字定义并通过构造函数初始化了一个complex类，这个类将不能被修改，只能读取属性。当使用者调用complex的real()或者imag()方法时，如果这两个方法在定义处没有加入const关键字，那么将报错。 

![img](./assets/dc93cccf1eb30f582bdc4c975e0adea0-100371.png)

### 参数传递和返回值 

參數傳遞:**pass by value vs. pass by reference (to const)** 

建议使用传引用，可以加const防止引用被修改。

**返回值傳遞:return by value vs. return by reference (to const)** 

返回值尽量用引用。

**friend (**友元**)** 

类声明中用friend修饰的（非成员）函数，可以通过对象访问这个对象的私有成员。

![img](./assets/219b5899ed7e38f334b8aa45f0d36f05-98356.png)

 <u>相同 class 的各個 objects 互為 friends (友元)</u> 

![img](./assets/ffe0fd2b081fb4354c2023fb88628859-95965.png)

### 操作符重载与临时对象

operator overloading (操作符重載-1, 成員函數) 

![img](./assets/163a13cf20ebbfc495f6638cd87a5bd0-112285.png)

class body 之外的各種定義 (definitions) 

![img](./assets/4c4a067b41ed702b47956c4656dde72f-60329.png)

operator overloading (操作符重載-2, 非成員函數) 

![img](./assets/d1e7a3d10ce9ce6bd519797748d87004-121530.png)

临时对象

![img](./assets/62f5c1287d70663add58b8dbcc138864-272191.png)

## 含指针的例子：string类

3个特殊函数

![img](./assets/57f328106678af074b6f4f9560fbac32-194714.png)



### 析构函数

<u>含指针的类，构造函数里多半要动态分配，必须在析构函数里手动释放。</u>

![img](./assets/4f20744481d923b9c34e8236fab107fc-103620.png)

<u>只要含有指针的类，必须实现拷贝构造和拷贝赋值函数。因为默认拷贝的是指针，而不是指针的内容。——浅拷贝。</u>

![img](./assets/45406f3e805afddc8b7e820c7503daab-228956.png)

### 拷贝构造

深拷贝 

![img](./assets/33cf7b502db857dd9bd93bb58783a94f-166720.png)

### 拷贝赋值

![img](./assets/6b2bf9e2ec7cc71986b3b0d9b0413ddf-465383.png)

### 所謂 stack (棧), 所謂 heap (堆) 

![img](./assets/af84187ac82785f171570cafd88fa9e7-124787.png)

**new**:先分配 **memory,** 再調用 **ctor** 

![img](./assets/8c2260bfae6f1ab54906a34fbb418cd8-116668.png)

面向对象的程序设计语言倾向于对象一定要经过初始化后，使用起来才比较安全。因此，引入了构造函数（constructor）的概念，用于对对象进行自动初始化。

> http://c.biancheng.net/view/149.html 
>
> C++ 析构函数问题？ - 邱昊宇的回答 - 知乎 https://www.zhihu.com/question/37762784/answer/73472884

**delete**:先調用 **dtor,** 再釋放 **memory** 

![img](./assets/d28f52eef22f0288d4de705377deae43-95587.png)

**array new** 一定要搭配 **array delete** 

![img](./assets/5d120864ac7fda991e679feb95753209-122507.png)

進一步補充:**static** 

static 成员变量必须在类声明的外部初始化，初始化时才分配内存。

static 成员变量既可以通过对象来访问，也可以通过类来访问。

静态成员函数只能访问静态成员数据、其他静态成员函数和类外部的其他函数。

> —— http://c.biancheng.net/cpp/biancheng/view/209.html 
>
> ——C++ 类中的static成员的初始化和特点 https://blog.csdn.net/men_wen/article/details/64443040 

![img](./assets/65be72d9f17d5368e8390f26dd6ac338-80533.png)

進一步補充: 把 ctors 放在 private 區 

![img](./assets/3600f8b45c6b482d189ce7c861097abb-45751.jpeg)

進一步補充:**function template,** 函數模板 

![img](./assets/af9fb92f96828ff03738c08d76061ec2-81693.png)

進一步補充:**namespace** 

![img](./assets/ac315afbe9d65b08d9dc25bf2b06435f-64720.png)

更多細節與深入 

![img](./assets/516b12c80fc360d3091534bc1e532fae-97486.png)

## 面向对象

•Inheritance (繼承) 

•Composition (複合) 

•Delegation (委托)

### Composition (複合), 表示 has-a 

**Adapter** 

![img](./assets/4bec02b9435d4b7a592e83c92aba856c-76392.png)![img](./assets/fc1095b0399b0a146859689129c96f7f-62658.png)

**Composition (**複合**)** 關係下的構造和析構 

![img](./assets/f22e76cd80daadcdd5c6ea1182d22ee9-97075.png)

### Delegation(委託). Composition by reference. 

pImpl：Handle-Body结构，可用于reference counting等。

![img](./assets/5b4da15438b5f41ec81cf99aba2608c5-106928.png)

### Inheritance (繼承), 表示 is-a 

![img](./assets/6995fa7e986a2d22d40b03a1e170434f-59323.png)

**Inheritance (**繼承**)** 關係下的構造和析構 

base class 的 dtor 必須是 virtual， 否則會出現 undefined behavior 

![img](./assets/274d235f9f08ac927a8135c03c5394bb-97931.png)

**Inheritance (**繼承**) with virtual functions (**虛函數**)** 

<u>只有虚函数可以被覆盖</u>

![img](./assets/24ed61c744a733e5fdbd06a099be41fd-109654.png)

**Inheritance+Composition** 關係下的構造和析構 

![img](./assets/6cdfa5735a4d406f072c6ceb5b7e1a21-76273.png)

**Delegation (**委託**) + Inheritance (**繼承**)** 

![img](./assets/6c3f26a74d1c76940f1757ca8f7318a1-205805.png)

subject-observer:每一个窗口都是一个observer，subject委托observer进行更新。

![img](./assets/b70db68f617c04add8fd18a8e75b0b2e-118481.png)

写一个文件/窗口系统，层级系统。该如何设计和关联class？

- Primitive ：文件
- Composite ：目录。

Composite 可以是Primitive的组合，也可以是自身的组合。为了实现这一点，为Composite和Primitive设计一个公共的父类Component。

![img](./assets/3940d0f78b910447fc17e10b1d35573e-112981.png)

Prototype

The Prototype design pattern solves problems like: 

- How can objects be created so that which objects to create can be specified at run-time?
- How can dynamically loaded classes be instantiated?

![img](./assets/b8331287c91b9b8d21bd49ba1ce2d90a-222878.png)

- 想要创建未来才会出现的子类（下面是派生的子类）。

- 子类中，安排一个静态对象（_LAST）,然后把它放到父类之前开辟出的一个空间中，这样父类就可以看到新创建的子类。
- 这个静态对象创建的时候，调用自己私有的构造函数，调用addPrototype，这样就把自己放到了父类中。
- 子类中，还需要准备一个clone函数。这样父类就可以通过调用clone方法来创建这种子类的副本。

Image代码：

![img](./assets/23ee14d6266cbcc76b649cd6b10f6ba4-296531.png)

LandSatImage代码：

![img](./assets/4d6c04a0a6542867426020dec6bb7801-373460.png)

# C++程序设计（下）

![img](./assets/346ecb7283ae308f7f3ec81e1d52f6b3-253298.jpeg)

## **转换函数**

conversion function

![img](./assets/1bc847e9e9b7f3050007a058b506b90c-191271.png)

**隐式转换**

![img](./assets/b80a1ee67c7edff9b6ede57eea962c26-205992.png)

同时存在conversion function和non-explicit-one-argument，编译器不知道取哪个（把分数转为double还是把double转为分数）

![img](./assets/2c098b79e9dbe4593599b67c0e8d24c0-211146.png)

**显式转换**：构造函数加上explicit。这样在上面两条路径中强制使用double转分数。

![img](./assets/8b9415b79b1c58d0a75c65235347d182-247051.png)

explicit使用场景比较少，主要就是这种场景。

## pointer-like classes

<u>智能指针 ：完成比指针更多的工作。一般都是包着一层普通指针。</u>

![img](./assets/ed9366adf190b142ec548a8d02118562-238126.png)

**关于迭代器**

<u>迭代器这种智能指针还需要处理++，--等符号。</u>

![img](./assets/57fa2a80e16914e0371cc7c918a5d6df-321152.png)

*表示取data

![img](./assets/47787a450625515307f3a809504a80c3-282058.png)

## function-like classes 仿函数

设计一个class，行为像一个函数，即仿函数。 重载()操作符，使得class调用得像function一样。 

![img](./assets/097b58245070e1efe9a6eb3c848b0017-305318.png)

**标准库中，仿函数使用的奇特的base classes**

为什么要继承这个，在标准库课程中再介绍。

![img](./assets/0a70dcc125a70e701d62b12c960fbf18-253946.jpeg)

## 模板

类模板、函数模板之前说过了。

### 成员模板

常用于标准库中，类是一个模板，其中的某个成员也是一个模板。例子就是，可以使用<鲫鱼，麻雀>对象来构造一个<鱼类，鸟类>的pair。

![img](./assets/58f36c78b1b98f75550d22a9df0dd76b-356503.png)

智能指针也是一样的设计，为了让子类指针构造父类指针对象。

![img](./assets/1821500f03f78474eecdef8ed25dce7a-104985.png)

### specialization特化

<u>模板是泛化，指定特定类型即特化</u>。模板为什么要特化，因为编译器认为，对于特定的类型，如果你能对某一功能更好的实现，那么就该听你的。

![img](./assets/defc978ba0441dff87ac5dbc350809fc-179772.png)

**偏特化**

第一种，模板参数个数上的偏特化（特化模板参数中的一个或多个）。第二种，范围上的偏特化（特化模板参数的范围，如把模板参数特化为指针）。

![img](./assets/7061ff2ad3b8d2f4a1f3ba5c8757df10-71317.jpeg)

### **模板模板参数**

模板中的一个模板参数也为模板。

![img](./assets/e45dcacb96892bfe9bcb63056b30fe07-143286.png)

智能指针的例子：

![img](./assets/2269126a285d88cf4ba50cfdbcc9c944-144890.png)

 

## 关于C++标准库

容器、迭代器、算法。

![img](./assets/662fcc4cbe1615a6c71309805a16ffe4-286911.png)

## C++11新特性

介绍3个内容，其他内容在C++ 11新课程中。[C++新标准C++11&14](侯捷-C++新标准C++11&14.md)

![img](./assets/073b278aaa13760aae1c20791d777dd7-202026.png)

### variadic templates 可变模板参数

![img](./assets/07d92c87c7650d32c8480d7f537c27e4-203366.png)

### auto关键字

语法糖，编译器自动匹配返回类型

![img](./assets/db3c80cc5bcf19040157d1729f8b96d0-106556.png)

### range-base for

使用单冒号来进行for循环遍历。

![img](./assets/468e1114cb36202e3e94e32b278d6177-121016.png)

### reference引用

![img](./assets/74f78fbdabde8a3f320b9532e313bb19-131974.png)

> 简谈 C++ 中指针与引用的底层实现 - 政子的文章 - 知乎 https://zhuanlan.zhihu.com/p/89175296 

引用通常用于参数类型和返回值类型，引用不是函数签名的一部分。

![img](./assets/02eb521519ea3d3c6a1667fdee2fc7b8-147733.png)

## 对象模型 Object Model

1. 语言中直接支持面向对象程序设计的部分
2. 对于各种支持的底层实现机制

> 参考：[C++对象模型 ](https://mikeblog.top/2019/02/15/C-对象模型/)

### vptr和vtbl

![img](./assets/3a14f7a2c1d8694735acb48965d47970-479838.png)

C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。



动态绑定：接口的实现由派生类完全覆盖。 就是说原本声明的类型是基类B,但是调用函数的时候执行的却是不同派生类(由初始化或者赋值的时候定义)D的函数。

比如，要定义一个不同子类的集合，由于集合中元素类型必须一样，因此只能转换为基类类型，但是调用这些元素的时候，是调用子类的函数。

![img](./assets/8ef232be11c57fe92e68b9d296d6ed01-24879.png)

动态绑定实现了多态，实现条件为

1. 通过基类的引用或指针调用
2. 调用的是虚函数

静态绑定和动态绑定的汇编实现

![img](./assets/a3e6318b5ee26f2726cebf6b1a9737d0-406956.jpeg)

### 关于this pointer

通过子类对象调用函数时，实际是把自己的this指针传递给函数，如果该函数是虚函数，则会通过动态绑定找到子类对象的实现函数。

![img](./assets/3bea256f2a2bd24b08a5f09258461ea6-149809.png)

## 浅谈const

当const成员函数和non-const成员函数同时存在的时候，const object只能调用const成员函数，non-const object只能调用non-const成员函数。

![img](./assets/25eaaa7d65f1ad40b8cffaeb24ca05a4-200542.png)

## 重载new、delete

<u>new和delete表达式无法重载，但是new和delete表达式调用的操作符（operator new、operator delete等）是可以重载的。</u>

![img](./assets/9f51f55b55ba403d478b9d81ea43f0ee-258279.png)

<u>重载全局operator new、operator delete比较危险，可以在成员函数中重载。</u>

**示例**

前面加上::表示强制使用全局函数（而不是成员函数）

![img](./assets/a4c59efc7d404abd1ffe13788605c57e-220397.png)

运行结果：

![img](./assets/051c83ab18def3b6e588a24b8d85f665-313435.png)

 

**重载new(), delete()**

placement new 定位new

![img](./assets/f2bd5da79a5893bccc708a06e65a0c86-157802.png)

代码

![img](./assets/dd8771580049fb52eaae2260e9db8ef7-494730.png)

标准库string重载placement new的例子：扩充申请量

![img](./assets/9690c31fea59529e62b5343e08c637f0-221153.png)