**typedef命令**

typedef命令用来为某个类型起别名。type代表类型名，name代表别名。

```
typedef type name;
```

typedef可以一次指定多个别名

```
typedef int antelope, bagel, mushroom; // 一次性为int类型起了三个别名
```

typedef可以为指针起别名。

```
typedef int* intptr; // intptr是int*的别名。这样在使用时不容易看出来，变量x是一个指针类型。
```

typedef也可以为数组类型起别名。

```
typedef int five_ints[5]; // five_ints是一个组数组，包含5个整数
five_ints x = {11, 22, 33, 44, 55};
```

typedef为函数起别名。

```
typedef signed char (*fp)(void); // 类型别名fp是一个指针，代表函数signed char (*)(void)
```

































