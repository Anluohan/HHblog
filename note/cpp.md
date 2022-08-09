# C++作死之旅

## vector

```cpp
#include<string>
#include<vector>
#include<iostream>
#include<cstdlib>  //C语言标准库
using namespace std;

int main() {
	int x = 1024;
	//指针代表特定内存地址，&x表示x对象的地址
	int* p = &x;

	cout << p << endl;  //地址
	cout << *p << endl; //所指的对象x


	vector<int> a, b, c, d;
	vector<int>* pv = 0;  //初始化指向0地址，即为空指针
	vector<int>* addrs[4] = {&a,&b,&c,&d};//vector<int>*类型的数组，其元素为指向vector的地址
	vector<int>* cur = 0;
	for (int i = 0; i < 4; i++) {
		cur = addrs[i];
		if (cur && !cur->empty() && ((*cur)[1] == 1)) {
			//*cur表示vector a，
		}
	}
	int index = rand() % 4;
	
	return 0;
}
```

## 指针

### 数组与指针

访问数组时，使用指针和下标一般是一样的。

下标操作实际上就是将arr的起始地址加上索引，从而找到该下标的值。

```cpp
arr[2] 与 *(arr + 2)相同

/*
 * 对于int arr[]
 * arr+2的地址是arr的起始地址加上两个整数元素的大小
 * int长度为4byte那么arr+2的结果就是arr的地址加上8
 * 如果arr的起始地址为1000，那么arr+2 = 1008
 */ 
```



### 指针函数

```cpp
/*
 * 函数指针，指向具有“相同参数和返回类型”的函数
 * 如下函数指针会指向const const vector<int>* func(int)这样的函数，函数名称可以不同。
 * 函数指针不是函数，是一个指针！
 */
const vector<int>* (*func_p)(int a);  
const vector<int>* (*func_p)(int a) = 0; //表示未指向任何函数
const vector<int>* (*func_p)(int a) = func //提供函数名即可获得函数的地址
//函数指针数组
const vector<int>* (*func_p_arr[])(int a) = {func_a,func_b,fun_c};
```

## 值传递&地址传递

```cpp
#include<string>
#include<vector>
#include<iostream>
using namespace std;

void swap(int &a, int &b);
void display(vector<int> v);   //值传递，复制一份，会影响效率，对于不需要修改的情况，可以添加const并且使用地址传递来避免被修改并提高效率
                               //void display(const vector<int> &v)
void sort(vector<int> &v);     //引用vector，否则只是复制一份vector排序 	

int main() {
	
	int arr[] = { 5,3,1,2,4 };
	vector<int> v(arr,arr + 5);

	sort(v);
	display(v);

	return 0;
}


void swap(int &a, int &b) {
	
	int temp = a;
	a = b;
	b = temp;
	
}

void display(vector<int> v)
{
	for (int i = 0; i < v.size(); i++) {
		cout << v[i] << endl;
	}
}

void sort(vector<int> &v)
{
	for (int i = 0; i < v.size() - 1; i++) {
		for (int j = 0; j < v.size() - 1 - i; j++) {
			if (v[j] > v[j + 1]) {
				swap(v[j], v[j + 1]);
			}
		}
	}

}
```

## 关键字

### const

表示不可变。

修饰变量时表示该变量为常数。

```cpp
//由于v不可变，因此返回v[i]的指针所对应的值也不可变，所以返回值需要用cosnt修饰*
int const* find(const vector<int> &v, int a) {

	for (int i = 0; i < v.size(); i++) {
		if (a == v[i]) {
			return &v[i];
		}
	}
	return 0;
}
```

函数后面加上const表示该函数只读成员变量，不能修改成员变量.

函数前面加上const表示该函数的返回值不能被修改

### explicit

将构造器生命为显式，禁止编译器优化进行隐式转换。也就是我们传进去什么就是什么。

### using

`using namespace xx;`:使用命名空间

`using Want = OHOS::AAFwk::Want;`定义别名

## 容器

### 定义方式

有五种定义方式

```cpp
#include<vector>
#include<deque>
#include<list>

//1.空容器
list<string> s_list;
vector<int> i_vec;

//2.特定大小容器，元素为默认值
list<string> s_list(1024);
vector<int> i_vec(1024);

//3.特定大小，并赋初始值
list<string> s_list(1024,"zhuhan")
vector<int> i_vec(1024,1);

//4. 通过迭代器iterator产生
int arr[3] = {1,2,3};
vector<int> i_vec(arr,arr+3);

//5. 根据某个容器复制
list<string> s_list(list);//将list复制给s_list 
```

### iterator

每个容器都提供了begin()的方法，可返回一个iterator，指向第一个元素。end()则返回最后一个元素

```cpp
//iterator定义,iter指向vector的第一个元素
vector<string>::iterator iter = vec.begin(); 
for(iter = vec.begin(); iter < vec.end(); iter++) {
    //
}
//对于const vector，需要定义const_iterator
//const_iterator不允许有写入操作
const vector<string> c_vec;
vector<string>::const_iterator c_iter = c_vec.begin(); 

//iterator调用所指元素提供的函数与指针一样采用：->
iter->size()  //显示string字符串长度

```

### map

map取值：int value = map[key];

map存值：map[key]++

```cpp
#include<map>
#include<string>

map<sring,int> words;
//遍历map
map<string,int>::iterator ite = words.begin();
for(;ite != words.end();ite++) {
	cout<<ite->first;  //key
	cout<<ite->second; //value
}
```

### set

##  面向对象

### 类作用域解析

```cpp
class:://表示类作用域解析
    
bool Stack::empty() {
    //empty()是Stack类的成员
}
```

### 多态

父类被继承后，如果父类的函数用virtual关键字修饰则在运行时才会决定调用子类的方法还是父类的方法。

```cpp
class Animal
{
public:
	//Speak函数就是虚函数
	//函数前面加上virtual关键字，变成虚函数，那么编译器在编译的时候就不能确定函数调用了。
	virtual void speak()
	{
		cout << "动物在说话" << endl;
	}
};
```

### 虚函数

```cpp
virtual void func() {
    //可以有实现
}
```

可以实现多态。

### 纯虚函数（抽象类）

当类中有了**纯虚函数**，这个类也称为抽象类。（**注意纯虚函数和虚函数是不一样的**）

纯虚函数语法：`virtual 返回值类型 函数名 （参数列表）= 0 ;`

**抽象类特点**：

- 无法实例化对象
- 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

```cpp
class Base
{
public:
	//纯虚函数
	//类中只要有一个纯虚函数就称为抽象类
	//抽象类无法实例化对象
	//子类必须重写父类中的纯虚函数，否则也属于抽象类
	virtual void func() = 0;
};

class Son :public Base
{
public:
	virtual void func() 
	{
		cout << "func调用" << endl;
	};
};

void test01()
{
	Base * base = NULL;
	//base = new Base; // 错误，抽象类无法实例化对象
	base = new Son;
	base->func();
	delete base;//记得销毁
}

int main() {

	test01();

	system("pause");

	return 0;
}

```

### 虚析构函数

多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码。需要通过虚析构函数来调用子类的析构函数来解决。 如果子类中没有堆区数据，可以不写为虚析构或纯虚析构。

虚析构函数：`virtual ~类名(){//body}`，可以自己实现，可以实例化对象

纯虚析构函数：也表示该类为抽象类

```cpp
virtual ~类名() = 0;//定义 
类名::~类名(){}		// 实现

virtual ~Animal() = 0;

Animal::~Animal()
{
	cout << "Animal 纯虚析构函数调用！" << endl;
}
```

#### 多态案例

**案例描述：**

电脑主要组成部件为 CPU（用于计算），显卡（用于显示），内存条（用于存储）

将每个零件封装出抽象基类，并且提供不同的厂商生产不同的零件，例如Intel厂商和Lenovo厂商

创建电脑类提供让电脑工作的函数，并且调用每个零件工作的接口

测试时组装三台不同的电脑进行工作。

```cpp
#include<iostream>
using namespace std;

//抽象CPU类
class CPU
{
public:
	//抽象的计算函数
	virtual void calculate() = 0;
};

//抽象显卡类
class VideoCard
{
public:
	//抽象的显示函数
	virtual void display() = 0;
};

//抽象内存条类
class Memory
{
public:
	//抽象的存储函数
	virtual void storage() = 0;
};

//电脑类
class Computer
{
public:
	Computer(CPU * cpu, VideoCard * vc, Memory * mem)
	{
		m_cpu = cpu;
		m_vc = vc;
		m_mem = mem;
	}

	//提供工作的函数
	void work()
	{
		//让零件工作起来，调用接口
		m_cpu->calculate();

		m_vc->display();

		m_mem->storage();
	}

	//提供析构函数 释放3个电脑零件
	~Computer()
	{

		//释放CPU零件
		if (m_cpu != NULL)
		{
			delete m_cpu;
			m_cpu = NULL;
		}

		//释放显卡零件
		if (m_vc != NULL)
		{
			delete m_vc;
			m_vc = NULL;
		}

		//释放内存条零件
		if (m_mem != NULL)
		{
			delete m_mem;
			m_mem = NULL;
		}
	}

private:

	CPU * m_cpu; //CPU的零件指针
	VideoCard * m_vc; //显卡零件指针
	Memory * m_mem; //内存条零件指针
};

//具体厂商
//Intel厂商
class IntelCPU :public CPU
{
public:
	virtual void calculate()
	{
		cout << "Intel的CPU开始计算了！" << endl;
	}
};

class IntelVideoCard :public VideoCard
{
public:
	virtual void display()
	{
		cout << "Intel的显卡开始显示了！" << endl;
	}
};

class IntelMemory :public Memory
{
public:
	virtual void storage()
	{
		cout << "Intel的内存条开始存储了！" << endl;
	}
};

//Lenovo厂商
class LenovoCPU :public CPU
{
public:
	virtual void calculate()
	{
		cout << "Lenovo的CPU开始计算了！" << endl;
	}
};

class LenovoVideoCard :public VideoCard
{
public:
	virtual void display()
	{
		cout << "Lenovo的显卡开始显示了！" << endl;
	}
};

class LenovoMemory :public Memory
{
public:
	virtual void storage()
	{
		cout << "Lenovo的内存条开始存储了！" << endl;
	}
};


void test01()
{
	//第一台电脑零件
	CPU * intelCpu = new IntelCPU;
	VideoCard * intelCard = new IntelVideoCard;
	Memory * intelMem = new IntelMemory;

	cout << "第一台电脑开始工作：" << endl;
	//创建第一台电脑
	Computer * computer1 = new Computer(intelCpu, intelCard, intelMem);
	computer1->work();
	delete computer1;

	cout << "-----------------------" << endl;
	cout << "第二台电脑开始工作：" << endl;
	//第二台电脑组装
	Computer * computer2 = new Computer(new LenovoCPU, new LenovoVideoCard, new LenovoMemory);;
	computer2->work();
	delete computer2;

	cout << "-----------------------" << endl;
	cout << "第三台电脑开始工作：" << endl;
	//第三台电脑组装
	Computer * computer3 = new Computer(new LenovoCPU, new IntelVideoCard, new LenovoMemory);;
	computer3->work();
	delete computer3;

}
```

## 模板

### 函数模板

C++的模板和Java泛型相似.

语法：typename可以用class代替.

```
template<typename T>
函数声明或定义
```

```cpp
//利用模板提供通用的交换函数
template<typename T>
void mySwap(T& a, T& b)
{
	T temp = a;
	a = b;
	b = temp;
}
```

### 类模板

创建一个通用的类，其成员类型为泛型T。

- 类模板不会自动类型推导，**因此在初始化的时候需要显示指定模板T的类型是什么。**
- 类模板可以设置默认属性。
- 类模板中的成员函数在调用时才创建，lazzy

```cpp
template<class NameType, class AgeType = int>
```

```cpp
#include <string>
//类模板
template<class NameType, class AgeType> 
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

//1、类模板没有自动类型推导的使用方式
void test01()
{
	// Person p("孙悟空", 1000); // 错误 类模板使用时候，不可以用自动类型推导
	Person <string ,int>p("孙悟空", 1000); //必须使用显示指定类型的方式，使用类模板
	p.showPerson();
}

```

- 类模板被继承时，子类需要指定父类中的T类型，否则无法分配内存，如果不想指定父类T的类型，那么子类也需要成为模板类。

```cpp
template<class T>
class Base
{
	T m;
};

//class Son:public Base  //错误，c++编译需要给子类分配内存，必须知道父类中T的类型才可以向下继承
class Son :public Base<int> //必须指定一个类型
{
};
void test01()
{
	Son c;
}

//类模板继承类模板 ,可以用T2指定父类中的T类型
template<class T1, class T2>
class Son2 :public Base<T2>
{
public:
	Son2()
	{
		cout << typeid(T1).name() << endl;
		cout << typeid(T2).name() << endl;
	}
};
```

## STL

### string

#### string和char \* 区别

- char * 是一个指针
- string是一个类，类内部封装了char*，管理这个字符串，是一个char*型的容器，管理char *所分配的内存.

str可以充当字符串数组和Java类似`str.charAt(i)`

```cpp
string str = "hello world";

	for (int i = 0; i < str.size(); i++)
	{
		cout << str[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < str.size(); i++)
	{
		cout << str.at(i) << " ";
	}
	cout << endl;


	//字符修改
	str[0] = 'x';
	str.at(1) = 'x';
	cout << str << endl;
```

#### 构造函数

1. `string();` //创建一个空的字符串 例如: string str;
   `string(const char* s);` //使用字符串s初始化
2. `string(const string& str);` //使用一个string对象初始化另一个string对象
3. `string(int n, char c);` //使用n个字符c初始化

```cpp
#include <string>
//string构造
void test01()
{
	string s1; 					//1.创建空字符串，调用无参构造函数
	cout << "str1 = " << s1 << endl;

	const char* str = "hello world";
	string s2(str); 			//1.把c_string转换成了string

	cout << "str2 = " << s2 << endl;

	string s3(s2); 				//2.调用拷贝构造函数
	cout << "str3 = " << s3 << endl;

	string s4(10, 'a'); 		//3.使用n个字符c初始化
	cout << "str3 = " << s3 << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}

```

#### 常用API

https://cplusplus.com/reference/string/string/

拼接字符串通常使用“+”和Java一样.

**长度**

```cpp
str.size(); //常用
str.length();
```

**添加**

- `string& append(const char *s);` //把字符串s连接到当前字符串结尾
- `string& append(const char *s, int n);` //把字符串s的前n个字符连接到当前字符串结尾
- `string& append(const string &s);` //同operator+=(const string& str)
- `string& append(const string &s, int pos, int n);`//字符串s中从pos开始的n个字符连接到字符串结尾

```cpp
str.append("zhuhan");
str.append("zhuhan",3);
str.append(str1);
str.append(str1,0,3);
```

**查找**

- string (1)	size_t find (const string& str, size_t pos = 0) const noexcept;
- c-string (2)	size_t find (const char* s, size_t pos = 0) const;
- buffer (3)	size_t find (const char* s, size_t pos, size_type n) const;
- character (4)	size_t find (char c, size_t pos = 0) const noexcept;

```
int pos = str1.find("de");  //返回找到的第一个字符串位置下标，没找到返回-1
```

### vector

#### 构造

```cpp
// constructors used in the same order as described above:
  std::vector<int> first;                                // empty vector of ints
  std::vector<int> second (4,100);                       // four ints with value 100
  std::vector<int> third (second.begin(),second.end());  // iterating through second
  std::vector<int> fourth (third);                       // a copy of third

  // the iterator constructor can also be used to construct from arrays:
  int myints[] = {16,2,77,29};
  std::vector<int> fifth (myints, myints + sizeof(myints) / sizeof(int) );  
```

#### 常用API

```cpp
v.push_back(ele); //尾插
v.insert(v.begin(),ele); //头插
v.insert(iterator,ele) //在迭代器位置插入元素	
```

### stack

方法名基本和Java差不多，但不完全一样。

```cpp
bool empty()//空则返回true,stack.empty();  

void pop(); //出栈，没有返回值！！

reference top(); //获取栈顶的引用
stack.top() = xx  //可以重新给栈顶赋值

void push()  //压栈

size_type size() const; //返回无符号整型
```

