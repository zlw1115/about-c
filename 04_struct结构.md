**struct结构**

struct关键字，允许自定义复合数据类型，将不同类型的值组合在一起。C语言没有对象（object）和类（class）的概念，struct结构很大程度上提供了对象和类的功能。

```
// 定义了一个分数的数据类型struct fraction，包含两个属性numerator和denominator
struct fraction{ // 类型名要包括struct关键字
  int numerator;
  int denominator;
}; // struct语句结尾的分号不能省略

// 声明该类型的变量
struct fraction f1; // 声明了一个struct fraction类型的变量f1，这是编译器就会为f1分配内存
f1.numerator = 22; // 为f1不同属性赋值，struct结构的属性通过.来表示
f1.denominator = 7;
```

声明了数据类型book和该类型的变量b1。

```
struct book{
  char title[500];
  char author[100];
  float value;
} b1;

// 如果类型标识符book只用在这一个地方，后面不再用到，这里可以将类型名省略
struct { // struct声明了一个匿名数据类型，又声明了这个类型的变量b1
  char title[500];
  char author[100];
  float value;
} b1;
```

在声明变量的同时，对变量赋值。

```
struct {
  char title[500];
  char author[100];
  float value;
} b1 = {"Harry Potter", "J. K. Rowling", 10.0},
b2 = {"Cancer Ward", "Aleksandr Solzhenitsyn", 7.85}
```

typedef命令可以为struct结构指定一个别名，使用起来更简洁

```
typedef struct cell_phone{
  int cell_no;
  float minutes_of_changes;
} phone;
phone p = {5551234, 5}
```

指针变量也可以指向struct结构

```
struct book{
  char title[500];
  char author[100];
  float values;
}* b1;
//或者写成两个语句
struct book{
  char title[500];
  char author[100];
  float value;
};
struct book* b1;
// 变量b1是一个指针，指向的数据是struct book类型的实例
```

struct结构占用的存储空间，不是各个属性存储空间的总和，而是最大内存占用属性的存储空间的倍数，其他属性会添加空位与之对齐。这样可以提高读写效率。

```
struct foo{
  int a;
  char* b;
  char c;
};
printf("%d\n", sizeof(struct foo)); // 24
```

struct有三个属性，在64为计算机上占用的存储空间分别是：int a占4个字节，指针char* b占8个字节，char c占1个字节，加起来一共是13个字节（4+8+1）。但是实际上，struct foo会占用24个字节，原因是它的最大的内存占用属性是char* b的8个字节，导致其他属性的存储空间也是8个字节，这样才可以对齐，导致整个struct foo就是24个字节。多出来的存储空间，都采用空位填充。

```
struct foo{
  int a;		// 4
  char pad1[4];	// 填充4字节
  char *b;		// 8
  char c;		// 1
  char pad2[7];	//填充7字节
}；

printf("%d\n", sizeof(struct foo)); // 24
```

为什么浪费这么多空间进行内存对齐呢？这是为了加快读写速度，把内存占用划分成等长区块，就可以快速在struct结构体中定位到每个属性的起始地址。

由于这个特性，在有必要的情况下，定义struct结构体时，可以采用存储空间递增的顺序，定义每个属性，这样就能节省一些空间。

```
struct foo{
  char c;
  int a;
  char* b;
};
printf("%d\n",sizeof(struct foo)); // 16
```

上面示例中，占用空间对小的char c排在第一位，其次是int a，占用空间最大的char* b排在最后。整个struct foo的内存占用就从24字节下降到16字节。

**struct的复制**

struct变量可以使用赋值运算符（=），复制给另一个变量，这时会生成一个全新的副本。系统会分配一块新的内存空间，大小与原来的变量相同，把每个属性都复制过去，即原样生成了一份数据。这一点跟数组的复制不一样（数组的复制是：两个指针指向同一个数组）。

复制运算符（=）可以将struct结构每个属性的值，一模一样复制一份，拷贝给另一个struct变量。这一点跟数组完全不同，使用赋值运算符复制数组，不会复制数据，指回共享地址。

注意：这种赋值要求两个变量是同一个类型，不同类型的struct变量无法互相赋值。另外，C语言没有提供比较两个自定义数据结构是否相等的方法，无法用比较运算符（比较==和!=）比较两个数据结构是否相等或不等。

**struct指针**

通常情况下，开发者希望传入函数的是同一份数据，函数内部修改i数据以后，会反映在函数外部。而且，传入的是同一份数据，也有利于提高程序性能。这是就需要将struct变量的指针传入函数，通过指针来修改struct属性，就可以影响到函数外部。

struct指针传入函数的写法：

```
void happy(struct turtle* t){
}
happy(&myTurtle)
```

上例中，t是struct结构的指针，调用函数时传入的是指针。struct类型跟数组不一样，类型标识符本身并不是指针，所以传入时，指针必须写成&myTurtle。

函数内部也必须使用(*)t.age的写法，从指针拿到struct结构本身。

```
void happy(struct turtle* t){
  (*t).age = (*t).age + 1;
}
```

(*t).age这样的写法很麻烦，C语言就引入了一个新的箭头运算符(->)，可以从struct指针上直接获取属性，大大增强了代码的可读性。

```
void happy(struct turtle* t){
  t->age = t->age + 1;
}
```

总结：对于struct变量名，使用点运算符(.)获取属性；对于struct变量指针，使用箭头运算符(->)获取属性。以变量myStruct为例，假设ptr是它的指针，下面写法相同：

```
// ptr == &myStruct
myStruct.pro == (*ptr).prop == ptr->prop
```

**struct的嵌套**

struct结构的成员可以是另一个struct结构。

```
struct species{
  char* name;
  int kinds;
};

struct fish{
  char* name;
  int age;
  struct specides breed;
}
```

struct结构内部不仅可以引用其他结构，还可以自我引用，即结构内部引用当前结构。比如，链表结构的节点就可以写成下面这样：

```
struct node{
  int data;
  struct node* next;
};
```

上面示例中，node结构的next属性，就是指向另一个node实例的指针。下面，使用这个结构自定义一个数据链表。

```
struct{
  int data;
  struct node* next;
}

struct node* head;

// 生成一个三个节点的列表(11)-> (22) -> (33)
head = malloc(sizeof(struct node));

head->data = 11;
head->next = malloc(sizeof(struct node));

head->next->data = 22;
head->next->next = malloc(sizeof(struct node))

head->next->next->data = 33;
head->next->next->next = NULL

// 遍历这个列表
for(struct node *cur = head; cur != NULL; cur = cur->next){
  printf("%d\n",cur->data)
}
```





























