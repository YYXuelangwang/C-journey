```
#include <stdio.h>
#include <iostream>
using namespace std;

int main(){
    
    /*
     static_cast<类型>()
        -转换时做静态检查，即在编译时进行
        -void*到其他指针的转换
     reinterprect_cast<类型>()
        -允许强转任何类型的指针
        -把整数强转成指针，指正强转成整数
     const_cast<类型>()
        -去掉cv限制
     dynamic_case<类型>()
        -动态转换
     */
    
    //编译器在这里发现 a 是const类型，他就认为 a 不可修改，所以在下面只能出现 a 他都会
    //去用200来替换
    
    //volatile 告诉编译器 const 类型变量，有可能进行修改，在使用时去内存中查找真实数据
    
    volatile const int a = 200;
    int *pa = const_cast<int*>(&a);
    *pa = 300;
    cout << "a =" << a << endl;
    
    
    return 0;
}
```
