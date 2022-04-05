## 符号表

### 变量名varTable, 结构体FieldList_

C--中的总的类型信息存储: 

```c
struct Type_
{
	enum { basic, array, structure, constant} kind;
	union
	{
		// 基本类型
		int basic;
		// 数组类型信息包括元素类型与数组大小构成
		struct { Type elem; int size; } array;
		// 结构体类型信息是一个链表
		Structure structure;
	} u;
};
```

结构体信息存储: 

- 结构体基本定义:

```c
struct Structure_
{
	char *name;          //结构体的名字
	FieldList strfield;    //结构体的域
};
```

- 结构体的域定义: 

```c
struct FieldList_
{
	char* name;	         // 域的名字
	Type type;	         // 域的类型
	FieldList tail;	     // 下一个域
	FieldList hashEqual;  //”name“的哈希值相同的构成一个链表
};
```



### 函数名funcTable, 结构体Functype_

函数信息存储

```c
struct Functype_
{
	char*name;           //函数的名字
	bool isDefined;      //是否已经被定义
	int row;             //位置信息
	Type ret_type;         //返回值类型
	FieldList param;     //参数链表
	Functype hashEqual;   //”name“的哈希值相同的构成一个链表
};
```



### 散列表

### 散列表冲突



## 类型表示

### Type_

### Structure_





## 重要函数

### paramEqual

### typeEqual

### printXXX

### insertFunc