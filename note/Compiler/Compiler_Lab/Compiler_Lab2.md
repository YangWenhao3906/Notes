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

#### 散列表接口

```c
/*actions on symbol table*/
unsigned int hash_pjw(char* name);//hash函数

void initTable();//初始化2个全局表格
int insertTable(FieldList f);//变量插入变量表
int insertFunc(Functype f,int type);//函数插入函数表
void insertParam(Functype f);//函数参数插入
FieldList findSymbol(char* name);//查找
Functype findFunc(char* name);//查找
void checkFunc();    //最后检验是否有未定义的函数


/*judgement */
bool paramEqual(FieldList f1,FieldList f2);
bool typeEqual(Type t1,Type y2);

/*print function */
void printparam(FieldList f);
void printargs(Node *n);
void printtype(Type t);
void printNode(Node* n);
```

#### 散列表插入

- 变量插入

```c
int insertTable(FieldList f)
{//分情况: hash表是否冲突
	if(f->name==NULL)return 0;	
	unsigned int no= hash_pjw(f->name);	//计算hash
	if(varTable[no]==NULL)	//若hash表不冲突=>直接存入
	{
		varTable[no]=f;
	}
	else //若hash表冲突=>检测是否重定义,挂在最后FieldList hashEqual
	{
		FieldList q=varTable[no];
		if(strcmp(q->name,f->name)==0)return 1;	     //检测到发生重定义 返回值为1
		while(q->hashEqual!=NULL)
		{//指针后移直至到链表末尾, 过程中也不断检测是否重定义
			q=q->hashEqual;
			if(strcmp(q->name,f->name)==0)return 1;	 //检测到发生重定义 返回值为1
		}
		q->hashEqual=f;
	}
	return 0;
}
```

- 函数插入: 函数插入函数表 + 参数插入变量表

## 类型表示

### Type_

### Structure_





## 重要函数

### paramEqual

### typeEqual

### printXXX

### insertFunc