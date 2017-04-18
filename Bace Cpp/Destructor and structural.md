```
class Date {
    int y, m, d;
public:
    Date(int y, int m, int d) : y(y), m(m), d(d){
        cout << "Date(int y, int m, int d) " << endl;
    }
    //对象销毁时会调用析构方法,如果我们自己没有
//    写析构方法，编译器会帮我们写一个然后调用
    ~Date(){
        cout << "~Date()" << endl;
    }
};

class Employee {
    string name;
    Date *birth;
public:
    Employee(string name) : name(name){
        
        birth = new Date(2000, 10, 15);
//        *birth = Date(1023, 3, 12);
        cout << "Employee(string name)" << endl;
    }
    //如果有成员变量是别的对象的指针，而且，这个指针指向的
//    对象是在本类中，在堆中开辟的空间创建的，并且该指针没有进行
//    过delete操作，那么就需要在析构方法中进行delete操作，此时
//    我们就需要自己写析构函数
    ~Employee(){
        delete birth;
        cout << "~Employee()" << endl;
    }
    
};

class Animal{
    string name;
    Date birth;
    Date *comDay;
public:
    Animal(string name, Date birth) : name(name), birth(birth){
        *comDay = Date(2013, 2, 12);
        cout << "Animal(string name, Date birth)" << endl;
    }
    
    
    ~Animal(){
        cout << "~Animal()" << endl;
    }
    
};


int main() {
    
    Employee em("zhangsan");
    
    
    
    return 0;
}


```
***
copyConstruct:拷贝构造
```
class Date {

    int y, m, d;
public:
    Date(){
        cout << "Date(int y, int m, int d) " << endl;
    }
    //拷贝构造 如果我们自己没写，编译器会帮我们写一个拷贝构造函数
    Date(const Date& d){
        cout << "拷贝构造函数 Date(Date& d)" << endl;
        this->y = d.y;
        this->m = d.m;
        this->d = d.d;
    }
    //对象销毁时会调用析构方法,如果我们自己没有
    //    写析构方法，编译器会帮我们写一个然后调用
    ~Date(){
        cout << "~Date()" << endl;
    }
    
};

class Employee {
    string name;
    Date *birth;
public:
    Employee(string name) : name(name){
        
        birth = new Date();
        
        //        *birth = Date(1023, 3, 12);
        cout << "Employee(string name)" << endl;
    }
   
    //如果一个类必须自己写析构方法的时候，拷贝构造方法也一定要自己写
    Employee(Employee& em){
        this->name = em.name;
        
        //这里不能使用浅拷贝，浅拷贝只能拷贝地址
        //this->birth = em.birth;
       // this->birth = new Date;
        
        this->birth = new Date(*em.birth);
        
        //this->birth->y = em.birth->y;
        //this->birth->m = em.birth->m;
        //this->birth->d = em.birth->d;
    }
    
    ~Employee(){
        delete birth;
        cout << "~Employee()" << endl;
    }
    
  //  int getY(){
  //      return birth->y;
 //   }
    
};

int main(){
    //const Date d;
   // Date d2 = d;
    
    Employee em("zhagne");
    Employee em2 = em;
    
   // em2 = em;
    
    return 0;
}

```
***
copyconstruct2 :拷贝构造2
```
class Date {
    
    int y, m, d;
public:
    Date(){
        cout << "Date(int y, int m, int d) " << endl;
    }
    //拷贝构造 如果我们自己没写，编译器会ibang我们写一个拷贝构造函数
    Date(const Date& d){
        cout << "拷贝构造函数 Date(Date& d)" << endl;
        this->y = d.y;
        this->m = d.m;
        this->d = d.d;
    }
    //对象销毁时会调用析构方法,如果我们自己没有
    //    写析构方法，编译器会帮我们写一个然后调用
    ~Date(){
        cout << "~Date()" << endl;
    }
    
};


void fa(Date d){}
void fb(Date &d){}
void fc(Date *d){ cout << d << endl; }
Date fd(){
    Date d;
    return d;
}
Date& fe(Date &d){
    return d;
}

Date ff(Date &d){
    return d;
}

int main(){
    Date d;
    cout << "1------------" << endl;
    fa(d);
    cout << "2------------" << endl;
    fb(d);
    cout << "3------------" << endl;
    fc(&d);
    cout << "4------------" << endl;
    Date d1 = fd();
    cout << "5------------" << endl;
    Date d2 = ff(d);
    cout << "6------------" << endl;
    Date &d3 = fe(d);
    cout << "7------------" << endl;
    
    return 0;
}
```
