
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
