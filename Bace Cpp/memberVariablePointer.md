```
//创建对象时，分配内存空间的大小是由该类的成员变量决定的

struct Date{
    
    //c 求结构体大小， （对齐补齐）
    //成员变量地址是相对对象的地址
    int year;
    int month;
    int day;
    void show(){
        cout << year << "" << endl;
    }
    
};

void print(Date *ds, int n, int Date:: *p) {
    
    for (int i = 0; i < n ; i ++) {
        cout << ds[i].*p << endl;
    }
    
}

struct Person{
    
    string firstName;
    string lastName;
    void eat(){
        cout << firstName << lastName << " person eat" << endl;
    }
};

//void run(Person *ps, )

typedef int Date::* f_bieming;
#define selector(SEL) (&Date::SEL)

void test(Date d, f_bieming p){
    cout << "d.*p = " << d.*p << endl;
}


int main(){
    
    int Date:: *p = &Date :: year;
    
    int Date:: *py = &Date :: year;
    int Date:: *pm = &Date :: month;
    int Date:: *pd = &Date :: day;
    
    cout << "py = " << py << endl;
    
    printf("%p", py);
    printf("%p", pm);
    printf("%p", pd);
    
    Date d;
    d.year = 2015;
    cout << d.year << endl;
    
    //通过.*访问成员变量，成员变量指针访问
    d.*py = 20.15;
    cout << d.year << endl;
    
    d.*pm = 1;
    d.show();
    
    Date *pd2 = new Date;
    pd2->year = 2015;
    pd2->*py = 300;
    
    Date ds[5] = {{2014,1,9}, {2011,2,12}, {2016,3,19}, {2015,8,19}, {2017,6,16},};
    print(ds, 5, &Date :: year);
    
    string Person:: *fn = &Person:: firstName;
    string Person:: *ln = &Person:: lastName;
    
    printf("%p\n", fn);
    printf("%p\n", ln);
    
    test({2013,3,14}, selector(day));
    
    return 0;
}
```

***

```

struct Date{
    
    //c 求结构体大小， （对齐补齐）
    //成员变量地址是相对对象的地址
    int year;
    int month;
    int day;
    void show(){
        cout << year << "" << endl;
    }
    void show2(int a, int b){
        cout << year << ' ' << month << ' ' << a + b << endl;
    }
    
    void showYear(){
        cout << "year = " << year << endl;
    }
    
    int add(){
        return year + month + day;
    }
    
    int sub(){
        return year - month - day;
    }
    
};

void test(int a){
    cout << "test" << endl;
}

typedef int (Date :: *FUNC4)();

void setFunc(Date d, FUNC4 f){
    cout << (d.*f)() << endl;
}

void subFunc(Date d){
    cout << d.sub() << endl;
}

typedef int T;

//给函数指针起别名
typedef void (*FUNC)(int) ;

typedef void (Date :: *FUNC2)();

typedef void (Date :: *FUNC3)(int, int);

#define selector(SET) (&Date::SET)

int main(){
    
    T a;
    //声明函数指针变量
    void (*f)(int);
    f = test;
    
    f(100);
    
    FUNC m;
    m = test;
    m(100);
    
    //获取成员函数地址
    //& 类名 :: 成员函数名称
//    &Date :: show;
    
    //声明成员函数指针
    //函数返回值类型 （类名 :: 成员函数指针变量名称) 参数
    
    void (Date :: *f2)();
    f2 = &Date :: show;
    
    Date d = {2015, 2, 23};
    d.show();
    
    (d.*f2)();
    
    void (Date :: *f3)(int, int);
    f3 = &Date :: show2;
    (d.*f3)(3, 4);
    
    FUNC2 f4 = &Date::showYear;
    (d.*f4)();
    
    subFunc(d);
    
//    setFunc(d, &Date::add);
//    setFunc(d, &Date::sub);

    setFunc(d, selector(add));
    setFunc(d, selector(sub));
    
    return 0;
    
}

```

****

```
class UIButton;

struct NSObject {
    bool init() {
        return true;
    }
};

struct Hero : public NSObject {
    void move(UIButton *button){
        cout << "hero hero " << endl;
    }
};

struct Plane : public NSObject{
    void fly(UIButton *button){
        cout << "plane plane " << endl;
    }
};

typedef void (NSObject::*FUNC)(UIButton *);

struct UIButton : public NSObject {
    private :
    int _tag;
    NSObject *_target;
    FUNC _func;
    public :
    static UIButton* buttonWithTarget(NSObject *target, FUNC func){
        UIButton *m_button = new UIButton;
        if (m_button->initWithTarget(target, func)) {
            return m_button;
        }
        delete m_button;
        return NULL;
    }
    
    bool initWithTarget(NSObject *target, FUNC func){
        if (!NSObject::init()) {
            return false;
        }
        _target = target;
        _func = func;
        return true;
    }
    
    int getTag(){
        return _tag;
    }
    
    void setTag(int tag){
        _tag = tag;
    }
    
    void touch(){
        (_target->*_func)(this);
    }
};

#define selector(SEL) (FUNC)(&SEL)

int main(){
    Hero *hero = new Hero;
    Plane *plane = new Plane;
    
    FUNC m = (FUNC)&Hero::move;
    
    UIButton *button = UIButton::buttonWithTarget(hero, selector(Hero::move));
    button->touch();
    return 0;
}

```
