
```
//C中 使用方法
//限制该变量只能在本文件中使用
static int x = 0;
//变量的作用于在函数中，但是是在程序开始时创建在
//全局区的，而不是在栈区
void test(){
    static int y = 0;
}

//C++中使用的方法
class Example {
public:
    int a;
    static int b;
    void show(){
        cout << "ageg = " << this->a << endl;
        
        cout << "b =" << b << endl;
        
        show2();
    }
    static void show2(){
        //静态函数中不可以访问成员变量
        //cout << "ageg = " << a << endl;
        
        //静态函数不可以访问成员函数
        //show();
        
        //静态函数可以访问静态变量
        cout << "b =" << b << endl;
    }
};

//类的变量开辟空间
int Example::b = 100;


int main(){
    
    Example ex;
    ex.a = 100;
    Example::b = 300;
    ex.show();
    Example ex2;
    ex2.a = 200;
    Example::b = 400;
    cout << Example::b << endl;
    
    Example::show2();
    return 0;
}

```
