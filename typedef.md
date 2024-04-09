typedef

- 为数组类型起别名：

  ```
  typedef int five_ints[5]
  five_ints x = {11, 22, 33, 44, 55}
  ```

  five_ints是一个数组类型，包含5个整数的。

- 为函数起别名：

```
typedef signed char (*fp)(void);
```

fp是一个函数指针，指向一个无参数，返回值为有符号字符值的函数

- 为struct、union、enum等命令定义的负责数据结构创建别名，从而便于引用。

  ```
  struct treenode{
    // ...
  };
  typedef struct treenode* Tree
  ```

  Tree为struct treenode*的别名

  typedef也可以与struct定义的数据类型的命令写在一起

  ```
  typedef struct animal{
    char* name;
    int leg_count, speed;
  } animal;
  ```

  上面示例中，自定义数据类型时，同时使用typedef命令，为struct animal起了一个别名animal。

  这种情况下，C语言允许省略struct命令后面的类型名。

  ```
  typedef struct {
    char *name;
    int leg_count, speed;
  } animal;
  ```

  





















