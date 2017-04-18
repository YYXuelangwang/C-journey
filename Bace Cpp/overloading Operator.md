
友元函数 或 成员函数重载operator
```
class F{
    int n;
    int d;
public:
    F(int n, int d) : n(n), d(d){}
    
    //友元函数，一个类的友元函数可以访问类中的私有变量
    friend ostream& operator<<(ostream &o, const F &f);
    
    //友元函数即使写在类中，但它依然是全局函数
    friend istream& operator>>(istream &i, F &f){
        i >> f.n >> f.d;
        return i;
    }
    
    //成员函数方式 重载+运算符函数
    F operator+(const F &f) const{
//        F n_f(n * f.d + d * f.n, d * f.d);
//        return n_f;
        
        //匿名对象
        return F(n * f.d + d * f.n, d * f.d);
    }
    
    F operator-(const F &f) const{
        return F(n * f.d - d * f.n, d * f.d);
    }
    
    F operator*(const F &f) const{
        return F(n * f.n, d * f.d);
    }
    
    F operator/(const F &f) const{
        return F(n * f.d, d * f.n);
    }
    
};

//class ostream{
//    void operator<<(int){}
//    void operator<<(double){}
//    void operator<<(char){}
//    
//};

//重载全局
ostream& operator<<(ostream &o, const F &f){
    cout << f.n << "/" << f.d;
    return o;
}



int main(int argc, const char * argv[]) {
    
    F f(2,3);
//    cout << f << endl;
    
//    cout.operator<<(f);
    //1，区ostreaml类中查看是否有满足的成员函数
    //2,查看是否有满足operator<<(ostream, F) 全局函数
    operator<<(cout, f);
    
    cout << f << endl;
    
    cin >> f;
    cout << f << endl;
    
    const F f2(3, 4);
    
    
    F f3 = f + f2;
    //1，查看成员函数
    f.operator+(f2);
    //2，查看友元函数
//    operator+(f, f2)
    
    return 0;
}

```
***
integer 的友元函数重载operator
```
class NSObject{
public:
    void retain(){}
};

class NSNumber : public NSObject {
    int x;
public:
    NSNumber(int x) : x(x){}
//    NSNumber(const char * s){}
    friend ostream& operator<<(ostream &o, const NSNumber &num) {
        
        o << num.x;
        return o;
    }
    
    //成员函数方式重载
    NSNumber operator+(const NSNumber &num) const{
        
        //构造方法的隐式类型转换
        return this->x + num.x;
    }
    
//    NSNumber operator*(const NSNumber &num) const{
//        return this->x * num.x;
//    }
//    
//    NSNumber operator/(const NSNumber &num) const {
//        return this->x / num.x;
//    }
//    
//    bool operator<(const NSNumber &num) const {
//        return this->x < num.x;
//    }
//    
//    bool operator>(const NSNumber &num) const{
//        return this->x > num.x;
//    }
//    
//    bool operator>=(const NSNumber &num) const {
//        return this->x >= num.x;
//    }
//    
//    bool operator<=(const NSNumber &num) const {
//        return this->x <= num.x;
//    }
    
    friend NSNumber operator*(const NSNumber &num1, const NSNumber &num2){
        return num1.x * num2.x;
    }
    
    friend NSNumber operator/(const NSNumber &num1, const NSNumber &num2){
        return num1.x / num2.x;
    }
    
    friend bool operator>(const NSNumber &num1, const NSNumber &num2){
        return num1.x > num2.x;
    }
    
    friend bool operator<(const NSNumber &num1, const NSNumber &num2){
        return num1.x < num2.x;
    }
    
    //友元函数的方式重载-号运算符
   friend NSNumber operator-(const NSNumber &num1, const NSNumber &num2){
        return num1.x - num2.x;
    }
    
    
    //单目运算符 - ! ~ ++ --
    NSNumber operator-(){
        return -x;
    }
    
    friend bool operator!(const NSNumber &num) {
        return !num.x;
    }
   
    NSNumber operator~(){
        return ~x;
    }
    
    NSNumber& operator++(){
        x++;
        return *this;
    }
    
    //带亚元参数的 ++ 为后++
    NSNumber operator++(int){
        NSNumber temp = *this;
        x++;
        return temp;
    }
    
    
    NSNumber& operator--(){
        x--;
        return *this;
    }
    
    NSNumber operator--(int){
        NSNumber temp = *this;
        x--;
        return temp;
    }
    
};




int main(){
    
    NSNumber num(100);
    cout << num << endl;
    
    NSNumber num2 = 100;
//    NSNumber num3 = "ssss";
    
    //双目运算符 友元函数方式重载会更好
    
    bool a = num > num2;
    
    bool is = !num2;
    cout << is << endl;
    cout << ~num2 << endl;
    
//    cout << ++++num2 << endl;
    NSNumber num3 = num2++;
    cout << num3 << endl;
    
    //如果多个后++，这时就不对了
    cout << num2 << num2++++ << endl;
    cout << num2 << endl;
    return 0;
}

```
***
重载赋值运算符 “=”
```
class Date {
    int y, m, d;
public:
    Date(int y, int m, int d) : y(y), m(m), d(d){
        cout << "Date(int y, int m, int d) " << endl;
    }
    Date(const Date &d){
        this->y = d.y;
        this->m = d.m;
        this->d = d.d;
        cout << "调用了Date(const Date &d)" << endl;
    }
    ~Date(){
        cout << "~Date()" << endl;
    }
    
    friend ostream& operator<<(ostream &o, const Date &d){
        
        o << d.y << ' ' << d.m << ' ' << d.d << endl;
        return o;
    }
    
    //赋值运算符函数 如果我们自己不写 编译器会帮我们完成
    Date& operator=(const Date &d){
        cout << "调用了赋值运算操作" << endl;
        this->y = d.y;
        this->m = d.m;
        this->d = d.d;
        return *this;
    }
};

int main() {
    
    Date d(2015, 12, 11);
    cout << d << endl;
    
    Date d2 = d;
    cout << d2 << endl;
    
    Date d3(2000, 13, 33);
//    d3 = d2;  //赋值操作
    d3.operator=(d2);
    
    
    cout << d3 << endl;
    
    return 0;
}
```
***
析构方法和赋值运算符重载
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
        
        cout << "Employee(string name)" << endl;
    }
    
    Employee(Employee& em){
        this->name = em.name;
        this->birth = new Date(*em.birth);

    }
    
    Employee& operator=(const Employee &em){
        //浅拷贝拷贝地址
        //        birth = em.birth;
        
        //1，判断是否是自赋值
        if (this == &em) return *this;
        
        //2,先释放原有空间
        delete birth;
        
        //3，再开辟新的空间
        birth = new Date(*em.birth);
        
        name = em.name;
        return *this;
    }
    
    ~Employee(){
        delete birth;
        cout << "~Employee()" << endl;
    }
    
    
};

int main() {
    
    Employee em("张三");
    Employee em2 = em;
    
    Employee em3("liso");
    
    //如果一个类中我们必须自己写析构函数，就必须自己
    //写拷贝构造和赋值运算符函数
    em3 = em;
    
    return 0;
}
```
