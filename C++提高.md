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

