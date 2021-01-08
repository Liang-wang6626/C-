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

自动类型推导，必须推导出一致的数据类型是才可以使用

模板必须确定T的数据类型才可使用