# C++中的构造函数

很久没有接触C++了，正好最近需要用到webkit的知识，发现已经多了很多新语法，总结一下构造函数的语法。

## 构造函数分类

- 显式构造函数：只带有一个必传参数的构造函数
- 默认构造函数
- 显示默认构造函数
- 多重继承类的构造函数
- 委托构造函数
- 继承构造函数

## 基本语法

### 构造函数顺序

1. 按照声明顺序调用基类和成员构造函数。当存在基类时，首先调用基类的构造函数，然后按照基类成员的声明顺序进行初始化并调用其类构造函数。当不存在基类时，先初始化成员，调用成员的构造函数，再调用类构造函数。如下代码示例调用顺序如下`Contained1->Contained2->BaseContainer->Contained3->DerivedContainer`

```C++
#include <iostream>  
using namespace std;  
  
class Contained1 {  
public:  
    Contained1() {  
        cout << "Contained1 constructor." << endl;  
    }  
};  
  
class Contained2 {  
public:  
    Contained2() {  
        cout << "Contained2 constructor." << endl;  
    }  
};  
  
class Contained3 {  
public:  
    Contained3() {  
        cout << "Contained3 constructor." << endl;  
    }  
};  
  
class BaseContainer {  
public:  
    BaseContainer() {  
        cout << "BaseContainer constructor." << endl;  
    }  
private:  
    Contained1 c1;  
    Contained2 c2;  
};  
  
class DerivedContainer : public BaseContainer {  
public:  
    DerivedContainer() : BaseContainer() {  
        cout << "DerivedContainer constructor." << endl;  
    }  
private:  
    Contained3 c3;  
};  
  
int main() {  
    DerivedContainer dc;  
    int x = 3;  
}  
```

### 成员列表

可以使用成员列表从构造函数参数来初始化类成员，比构造函数体内使用赋值运算符更高效，使用`constructor():memberA(val),memberB(val)...`语法进行赋值。示例如下：

```C++

class Box {  
public:  
    Box(int width, int length, int height)   
        : m_width(width), m_length(length), m_height(height) 
    {}  
    int Volume() {return m_width * m_length * m_height; }  
private:  
    int m_width;  
    int m_length;  
    int m_height;  
  
}

```


## 进阶使用

## 总结



