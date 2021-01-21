### 函数模板

#### 函数模板的基本语法

```c++
template<typename T>   template<class T>
void swap(T &a,T &b)
{
    T temp;
    temp=a;
    a=b;
    b=temp;
}
int main()
{
    swap(a,b)
    swap<int>(a,b)//手动指定传入的数据类型
}
```





#### 函数模板的注意事项

- 自动类型推导，必须推导出一致的数据类型是才可以使用

- 模板必须确定T的数据类型才可使用







#### 普通函数与模板函数

- 普通函数可以进行隐式类型转换
- 模板函数使用自动类型推导不可以使用隐式类型转换
- 模板函数使用确定类型时可以使用隐式类型转换

```c++
int int_add(int a,int b)
{
    return a+b ;
}
template <typename T>
int T_add(T a,T b)
{
    return a+b ;
}
int main()
{
    int a,b;
    char c;
    int_add(a,c);   //可以发生隐式转换
    T_add(a,c);     //不可以
    T_add<int>(a,c);//可以
}
```







#### 普通函数和模板函数的调用规则

- 如果普通函数和模板函数都可以实现，优先调用普通函数
- 可以通过空模板参数列表来强制调用函数模板
- 函数模板也可以发生重载
- 如果函数模板可以产生更好的匹配，优先调用函数模板





#### 模板的局限性

可以自定义模板要传入的数据类型

```c++
#include<iostream>
#include<string>
using namespace std ;

class person
{
    public:
        string m_name;
        int m_age;
    person(string name,int age)
    {
        this->m_name = name ;
        this->m_age = age ;
    }
};
template<typename T>
bool equal(T a,T b)
{
    if(a == b)
    {
        return true ;
    }
    else
    {
        return false ;
    } 
}

template<>bool equal(person p1,person p2)
{
    if(p1.m_age == p2.m_age && p1.m_name == p2.m_name)
    {
        return true ;
    }
    else
    {
        return false ;
    }
    
}

int main()
{
    int a = 10 ;
    int b = 10 ;
    person p1("Aliace",10);
    person p2("Bob",10);
    
    bool result = equal(p1,p2);
    if(result)
    {
        cout << "相等" << endl ;
    }
    else
    {
        cout << "不相等" << endl ;
    }
    

    system("pause");
    return 0;
}
```





















### 类模板

#### 类模板的基本语法

```c++
template<class nameType,class ageType>
class person
{
    public:
        nameType m_name ;
        ageType m_age ;
    person(nameType name,ageType age)
    {
        this->m_age = age ;
        this->m_name = name ;
    }
};

int main()
{
    person<string,int>p1("Aliace",10);
    cout << p1.m_age<< endl ;
    cout << p1.m_name << endl ;

    system("pause");
    return 0 ;
}

```





#### 类模板与函数模板的区别

- 类模板没有自动类型推导

- 类模板可以有默认参数

  ```c++
  template<class nameType,class ageType = int> //可以有默认参数
  class person
  {
      public:
          nameType m_name ;
          ageType m_age ;
      person(nameType name,ageType age)
      {
          this->m_age =age ;
          this->m_name = name ;
      }
      void show()
      {
          cout << "name : " << this->m_name << endl ;
          cout << "age : " << this->m_age << endl ;
      }
  };
  int main()
  {
      person<string>p1("Aliace",100); //如果有默认参数，可在创建对象是不用再写类型
      p1.show();
      system("pause");
      return 0;
  }
  ```

  





#### 类模板中成员函数创建的时机

类模板和普通成员函数的区别：

- 普通成员函数一开始就创建。
- 类模板的成员函数在调用时才创建。









#### 类模板对象做函数参数

- 指定类型传入——直接显示对象的数据类型
- 参数模板化——将对象的参数进行模板化进行传入
- 整个类模板化——将对象模板化进行传递

```c++
template<class nameType,class ageType>
class person
{
    public:
        nameType m_name ;
        ageType m_age ;
    person(nameType name,ageType age)
    {
        this->m_age =age ;
        this->m_name = name ;
    }
    void show()
    {
        cout << "name : " << this->m_name << endl ;
        cout << "age : " << this->m_age << endl ;
    }
};

void test01(person<string,int> &p)     //指定类型传入——直接显示对象的数据类型
{
    p.show();
}

template<class nameType,class ageType>     //参数模板化——将对象的参数进行模板化进行传入
void test02(person<nameType,ageType> &p)
{
    p.show();
}

template<class T>                       //整个类模板化——将对象模板化进行传递
void test03(T &p)
{
    p.show();
}
int main()
{
    person<string,int>p1("Aliace",100);
    person<string,int>p2("Bob",200);
    person<string,int>p3("Chris",300);
    test01(p1);
    test02(p2);
    test03(p3);
    system("pause");
    return 0;
}
```













#### 类模板与继承

- 子类继承的父类是一个模板时，子类在声明时，要指定父类中T的类型
- 如果不指定，编译器无法给子类分配内存
- 如果想要灵活指定父类中T的类型，子类也需要变为类模板

```c++
template<class BaseType>
class Base
{
    public:
        BaseType m;

};

template<class SonType,class BaseType>
class Son : public Base<BaseType>
{
    public:
        SonType m ;
    Son()
    {
        cout << typeid(SonType).name() << endl ;
        cout << typeid(BaseType).name() << endl ;
    }
};

void test()
{
    Son <int ,char>s ;
}

int main()
{
    test();
   
    system("pause");
    return 0;
}
```











#### 类模板成员函数的类外实现

```c++
template<class T1,class T2>
class person
{
    public:
        T1 m_name ;
        T2 m_age ;
    person(T1 name,T2 age);
    void show();
};

template<class T1,class T2>
person<T1,T2> :: person(T1 name,T2 age)
{
    this->m_age = age ;
    this->m_name = name ;
}

template<class T1,class T2>
void person<T1,T2> :: show()
{
    cout << this->m_age << endl ;
    cout << this->m_name << endl ;
}

int main()
{
    person<string,int>p("Aliace",100);
    p.show();
    system("pause");
    return 0 ;
}

```



















### STL

#### STL的基本概念

STL(Standard Template Library,标准模板库)



#### STL的六大组件

- 容器，算法，迭代器，仿函数，适配器（配接器），空间配置器
- 容器：各种数据结构如vector,list,deque,set,map,用来存放数据
- 算法：各种常用的算法，如sort,find,copy,for_each等
- 迭代器：扮演了容器与算法之间的粘合剂
- 仿函数：行为类似函数，可作为算法的某种策略
- 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西
- 空间适配器：负责空间适配与管理





#### 迭代器

|      种类      |         功能         |                                    |
| :------------: | :------------------: | :--------------------------------: |
|   输入迭代器   |         只读         |       只读，支持++，==，！=        |
|   输出迭代器   |         只写         |            只写，支持++            |
|   前向迭代器   | 读写，向前推进迭代器 |       读写，支持++，==，！=        |
|   双向迭代器   | 读写，向前和向后操作 |          读写，支持++，--          |
| 随机访问迭代器 |  读写，跳跃访问数据  | 读写，支持++，--，[n],-n,<,<=,>,>= |





#### vector存放内置数据类型

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std ;

//第一种方法
void test_01() 
{
    vector<int> v ;
    v.push_back(10);   //插入数据
    v.push_back(20);
    v.push_back(30);
    vector<int>::iterator itBegin = v.begin();  //起始迭代器，指向第一个元素
    vector<int>::iterator itEnd = v.end() ;     //结束迭代器，指向最后一个元素的下一个元素
    while(itBegin != itEnd)
    {
        cout << *itBegin << endl ;
        itBegin++ ;
    }
}

//第二种方法
void test_02()
{
    vector<int> v ;
    v.push_back(10);
    v.push_back(20);
    v.push_back(30);
    for(vector<int>::iterator itBegin=v.begin();itBegin < v.end();itBegin++)
    {
        cout << *itBegin << endl ;
    }
}

void my_prient(int val)
{
    cout << val << endl ;
}
//第三种方法
void test_03()
{
    vector<int> v ;
    v.push_back(10);   //插入数据
    v.push_back(20);
    v.push_back(30);
    for_each(v.begin(),v.end(),my_prient); //利用系统提供的遍历函数，包含头文件#include<algorthm>
   
}

int main()
{
    //test_01();
    //test_02();
    test_03();
    system("pause");
    return 0 ;
}
```







#### vector存放自定义数据类型

```c++
#include<iostream>
#include<string>
#include<vector>
using namespace std ;

class person
{
    public:
        string m_name;
        int m_age ;
    public:
        person(string name,int age)
        {
            this->m_age = age ;
            this->m_name = name ;
        }
};

void test_01()
{
    vector<person>v;
    person p1("Aliace",10);
    person p2("Bob",20);
    person p3("Crist",30);
    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    for(vector<person>::iterator itBegin =v.begin();itBegin != v.end();itBegin++)
    {
        cout<< itBegin->m_name << endl ;
        cout<< itBegin->m_age << endl ;

    }
}

int main()
{
    test_01();
    system("pause");
    return 0 ;
}
```





















### string容器

#### string构造函数

```c++
// string()  默认构造函数
// string(const char * s) 使用字符串初始化
// string(const string &str) 将一个字符串对像赋给另一个字符串对象
// string(int n,char c) 使用n给字符c初始化

#include<iostream>
#include<string>
using namespace std ;

void test()
{
    string s1("Hello World!");
    cout << "s1 = : "<< s1 << endl ;
    const char*ptr = "Hello,world";
    string s2(ptr);
    cout << "s2 = : "<< s2 << endl ;
    string s3 (s2);
    cout << "s3 = : "<< s3 << endl ;
    string s4(5,'a');
    cout << "s4 = : "<< s4 << endl ;
}

int main()
{
    test();
    system("pause");
    return 0 ;
}
```







#### string的赋值操作

```c++
//string的赋值操作

#include<iostream>
#include<string>
using namespace std ;

void test()
{
    string str1;
    str1 = "Hello World!";
    cout << "str1 = "<< str1 << endl ;

    string str2 = str1 ;
    cout << "str2 = " << str2 << endl ;

    string str3 ;
    str3.assign("Hello World!");
    cout << "str3 = " << str3 << endl ;

    string str4 ;
    str4.assign("Hello world!",5);
    cout << "str4 = " << str4 << endl ;

    string str5;
    str5.assign(str4);
    cout <<"str5 = " << str5 << endl ;

    string str6 ;
    str6.assign(5,'a');
    cout <<"str6 = " << str6 << endl ;
    

}

int main()
{
    test();
    system("pause");
    return 0 ;
}
```













#### string的字符串拼接

```c++
void test()
{
    string str1 ("Hello");
    string str2 = "World!" ;
    str1 = str1 + str2 ;
    cout << "str1 = " << str1 << endl ;
    cout << "str2 = " << str2 << endl ;

    string str3 = "I love C++" ;                  //string& apend(const char* s)
    str3.append("Hello");
    cout << "str3 : " << str3 << endl ;

    str3.append(str2);
    cout << "str3 : " << str3 << endl ;          //string& append(const string& str)

    string str4 = "I love" ;
    str4.append("Hello",3);
    cout << "str4 : " << str4 << endl ;         //string& append(const char* s,int n)将字符串s的前n个字符拼接

    string str5 = "Hello" ;
    str5.append(str3,7,3);
    cout << "str5 : " << str5 << endl ;        //string& append(const char*s ,int n1,int n2)
                                               //从字符串的n1位置开始n2个字符进行拼接
}
```









#### string 字符串的查找与替换

```c++
int find(const string& str,int pos)//查找str第一次出现的位置，从pos开始,pos默认为0
int find(const char* s,int pos )//查找s第一次出现的位置，从pos开始，pos默认为0
int find(const string& str ,int pos,int n)//查找前n个字符
int rfind(const string& str,int pos)//与find不同的是，rfind从右往左开始查找
string& replace(int pos,int n,string& str)//从pos开始，替换n个str字符
    
void test()
{
    string str1 = "Hello world!" ;
    int pos = str1.find('o');
    cout << pos << endl ;

    pos = str1.rfind('o');
    cout << pos << endl ;

    pos = str1.find("o",1,2);
    cout << pos << endl ;

    str1.replace(1,2,"8");
    cout << str1 << endl ;
}
```













#### string字符串的比较

*等于返回0，大于返回1，小于返回-1*

```c++
int compare (const string& str)
int compare (const char* s)
    
void test()
{
    string str1 = "Hello" ;
    string str2 = "Hello" ;
    if(str1.compare(str2) == 0)                //等于返回0，大于返回1，小于返回-1
    {
        cout << "str1 = str 2" << endl ;
    }
    else if(str1.compare(str2) > 0 )
    {
        cout << "str1 > str2" << endl ;
    }
    else
    {
        cout <<"str1 < str2 " << endl ;
    }
}
```













#### string单个字符的存取

- 通过[]方式访问
- 通过at方式访问

```c++
void test()
{
    string  str1 = "Hello" ;
    for(int i =0 ;i<str1.size() ;i++)         //[]方式
    {
        cout << str1[i] << " " ;
    }
    cout << endl ;

    for(int i =0 ;i < str1.size() ;i++)       //at方式
    {
        cout << str1.at(i) << " " ;
    }
    cout << endl ;

    str1[0] = 'W' ;
    str1.at(1) = 'W' ;
    cout << "str1 = " << str1 << endl ;
}
```













#### string插入和删除

```c++
string &insert(int pos,const string& str)
string &insert(int pos,const char* s)
string &erase(int pos,int n)
    
void test()
{
    string str = "Hello" ;
    str.insert(1,"aaa");
    cout << "str = " << str << endl ;
    str.erase(1,3);
    cout << "str = " << str << endl ;
}

```









#### string子串

```c++
void test()
{
    string str = "Hello" ;
    string s = str.substr(2,2) ;   //string substr(int pos,int n)从pos位置开始,截取n个字符
    cout << "str = " << str << endl ;
    cout << "s = " << s << endl ;
}
```

































### vector容器

#### 基本概念

vector与数组非常类似，是单端数组

与数组的区别：
**数组时静态内存，而vector是动态可扩展的**

**可扩展的意思是寻找一块更大的内存，然后将原来的数据拷贝到该内存空间下，将原来的空间释放掉**

```c++
void printVector(vector<int>&v)
{
    for(vector<int>::iterator it =v.begin();it != v.end();it++)
    {
        cout << (*it) << " ";
    }
    cout << endl ;
}

void test()
{
    vector<int>v1 ;                 //默认构造函数，无参
    for(int i =0 ;i < 5;i++)
    {
        v1.push_back(i);
    } 
    printVector(v1);

    vector<int>v2(v1);
    printVector(v2);             //拷贝构造函数

    vector<int>v3(v1.begin(),v1.end());    //区间法
    printVector(v3);

    vector<int>v4(5,10);
    printVector(v4);                    //n个elem
}
```









#### vector赋值

```   c++
vector<int>v1;
vector<int>v2;
v2 = v1 ;               //赋值
v2.assign(begin,end)    //区间
v2.assign(n,elem)       //n 个elem
```









#### vector的容量和大小

```c++
void test()
{
    vector<int>v;
    for(int i = 0;i<7;i++)
    {
        v.push_back(i);
    }
    printVector(v);

    bool b = v.empty() ;                  //为空则返回ture,不为空则返回false
    cout << "bool b : " << b << endl ;

    cout << "v.capacity : " << v.capacity() << endl ;    //容量
    cout << "v.size : " << v.size() << endl ;           //大小

    v.resize(9) ;   //重新指定大小，扩展后的用数字0填充
    printVector(v);
    cout << "v.capacity : " << v.capacity() << endl ;    //容量

    v.resize(12,66);  ////重新指定大小，扩展后的用指定数字填充
    printVector(v);

}

```









#### vector的插入和删除



```c++
/*
push_back()                                       //尾插
pop_back()                                        //尾删
insert(const_iterator pos,elem)                  //在迭代器指定位置上插入元素，默认为0
insert(const_iterator pos,int n,elem)            //在迭代器指定位置上插入n个elem元素
erase(const_iterator pos)                        //删除迭代器指定位置的元素
erase(const_iterator start,const_iterator end)   //删除start到end区间内的元素
clear()                                          //删除所有元素
*/
void test()
{
    vector<int>v ;
    for(int i=0 ;i < 6 ;i++)
    {
        v.push_back(i) ;
    }
    printVector(v);

    v.insert((v.begin()+1),66);    //在指定位置上插入，指定位置使用迭代器
    printVector(v);

    v.insert(v.begin(),2,100);     //在指定位置上插入n个elem
    printVector(v);

    v.erase(v.begin());            //指定位置删除某个数，位置使用迭代器指定
    printVector(v);

    v.erase((v.begin()+2),(v.end()-2)); //删除从[n1,n2]区间内的元素
    printVector(v);

    v.pop_back();                     //删除尾部最后一个元素
    v.clear();                        //删除所有元素
    printVector(v);
}
```













#### vector数据的存取

```c++
void test()
{
    vector<int>v ;
    for(int i =0 ;i < 6 ; i++)
    {
        v.push_back(i);
    }

    for(int i=0 ;i<v.size();i++)
    {
        cout << v[i] << " " ;        //通过[]方式访问
    }
    cout << endl ;

    cout <<v.at(2) << endl ;        //通过at方式访问

    cout << v.front() << endl ;     //front()返回第一个元素
    cout << v.back() << endl ;      //back()返回最后一个元素
}
```





#### vector互换容器

swap();



#### vector预留空间

reserve(int len)预留空间，预留空间中的元素不可访问

























### deque容器

#### 基本概念

- 双端数组，可以对头端进行插入和删除操作

deque与vector的区别：

- vector对于头部的插入效率低，数据量越大，效率越低
- deque相对而言，对头部的插入和删除速度比vector快
- deque访问元素时的速度会比vector慢







#### deque构造函数

deque<T>           

deque(int n,elem)  n个elem

deque(begin,end)区间

deque(deque &)拷贝构造函数











#### deque赋值

- operator+(deque &)
- assign(begin,end)
- assign(int n,elem)







#### deque的容量和大小

和vector一样

deque.empty()判断是否为空

deque.size()容器的大小

deque.resize(n)重新指定大小，指定空间大于原来的空间则默认用0填充，小于原来的空间则依次删除尾部

deque.resize(n,elem)指定元素填充







#### deque的插入和删除

两端操作：

- push_back(elem)尾插
- push_front(elem)头插
- pop_back()尾删
- pop_front()头删

指定位置：

insert(pos,elem)利用迭代器指定位置pos，然后在该位置插入数据elem，返回新数据的位置

insert(pos,n,elem)在pos位置插入n个elem数据，无返回

insert(pos,begin,end)在pos位置上插入区间[begin,end)，无返回

clear()清除所有元素

erase(begin,end)删除[begin,end)区间的数据，返回下一个元素的位置

erase(pos)删除指定位置上的元素，返回下一个元素的位置





#### deque的数据存取

- front()取头部元素
- back()去尾部数据
- deque.at()
- operator[]

















### stack容器



#### 常用接口

构造函数

- stack<T>stk;
- stack(const stack & stk)   拷贝构造函数

赋值操作

- stack& operator = (const stack& stk)   重载等号赋值操作

数据存取

- stack.push()
- stack.pop()
- stack.top()返回栈顶元素

大小操作

- stack.empty()
- stack.size()















### queue容器

#### 常用接口

构造函数

- queue<T>que;
- queue(const queue & stk)   拷贝构造函数

赋值操作

- queue& operator = (const queue& que)   重载等号赋值操作

数据存取

- queuepush()
- queue.pop()
- queue.front()   对头元素
- queue.back()  队尾元素

大小操作

- queue.empty()
- queue.size()





















### list容器

#### 基本概念

STL中list是一个双端循环链表

由于链表的储存空间并不是连续的内存空间，因此链表list中的迭代器只支持前移和后移

list的优点：

- 采用动态内存分配，不会造成资源的浪费与溢出
- 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

listd的缺点：

- 链表灵活，但是空间（指针域）和时间（遍历）需要耗费较大

list的一个重大性质就是，插入操作和删除操作都不会造成原有list容器的失效









#### list构造函数

- list<T>L;
- list(const list &)
- llist(n,elem)
- list(begin,end)







#### list赋值和交换

- list &operator=(const list &L) 重载=赋值
- assign(begin,end)  区间
- assign(n,elem)
- swap(const list & L)





#### list容器的大小操作

- empty()判断是否为空

- size()容器的大小

- resize(n)重新指定大小，指定空间大于原来的空间则默认用0填充，小于原来的空间则依次删除尾部

- resize(n,elem)指定元素填充










#### list容器的插入和删除

- push_front(n)
- pop_front()
- push_back(n)
- pop_back()
- insert(pos,elem)  insert(pos,n,elem)
- insert(pos,begin,end)
- clear()
- erase(pos,begin,end)
- erase(pos)
- remove(elem)移除所有与elem相同的元素







#### list数据的存取

- front() 返回头部元素
- back() 返回尾部元素









#### list的反转和排序

- reverse()  反转链表
- list.sort()  链表的排序     成员函数

















### set/multiset容器

#### 基本概念

简介：

- 所有元素在插入时都会自动被排好序，set/multiset属于关联式容器，底层结构是用二叉树实现的



set与multiset的区别：

- set不允许有重复的元素
- multiset可以有重复的元素









#### set容器的构造和赋值

```c++
set<T>s;     //默认构造
s.insert();  //set插入数据只有insert，而且不存在重复的数
set &operator=(const set& s); //重载等于=号赋值
set& (const set& s)  //拷贝构造函数
```







#### set大小和交换

- s1.size()
- s1.empty()是否为空
- s1.swap(s2)交换两个容器集合





#### set容器的插入和删除

- insert(elem)
- clear()
- erase(elem)
- erase(pos)
- erase(begin,end)





#### set查找和统计

- find()
- count()

```c++
void test()
{
    set<int>s;
    for(int i=1 ;i<=4;i++)
    {
        s.insert(i);
    }
    printSet(s);

    set<int>::iterator xs = s.find(3) ;    // 找到元素，返回迭代器的位置
    cout << *xs << endl ;
    xs = s.find(5);                        //没有找到元素，则返回end()元素位置
    cout << *xs <<endl ;

    int temp = s.count(1);                //count()查找共有几个相似的元素
    cout << temp << endl ;                //对于set容器只有0和1，对于multiset容器可以有多种值
}
```





#### pair对组

简介：

- 成对出现的数据，利用对组可以返回两个数据

两种创建方式：

- pair<T,T>p(value,value)
- pair<T,T>p=make_pair(value,value)













### map/multimap容器



#### map的基本概念

简介：

- map中所有元素都是pair
- pair中的第一个元素为key值，起到索引作用，第二个元素为value(实值)
- 所有元素都会根据键值(key)自动排序

本质：

- map/multimap属于关联式容器，底层结构是用二叉树来实现的

map与multimap的区别：

- map不允许容器中有重复的key值元素，可以有重复的value
- multimap中允许容器有重复的key值元素







#### map容器的构造和赋值

```c++
map<T,T> m ;   //默认构造
map& operator=(const map& ) //重载等号
map &(const map& ) //拷贝构造
map<int ,int> m ;
m.insert(pair<int,int>(1,10));
m.insert(pair<int,int>(2,20));
```







#### map容器的大小和交换

- size()
- empty()
- swap()





