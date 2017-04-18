```
//class Type{}
//Type object
//object() 函数对象
//object.operatro()
//object(2)

void add5(int &r){
    r += 5;
}

void add10(int &r){
    r += 10;
}

void foreach_5(int *a, int n, void (*f)(int &r)){
    for (int i = 0; i < n; i++) {
        f(a[i]);
    }
}

void print(int &r){
    cout << r << ' ';
}

class Add{
    int x;
public:
    Add(int x) : x(x){}
    void operator()(int &r){
        r += x;
    }
};

void for_each(int *a, int n, Add add){
    for (int i = 0; i < n; i++) {
//        add.operator()(a[i]);
        add(a[i]);
    }
}

void for_each1(int *a, int n, std::function<void(int&)> func){
    for (int i = 0; i < n; i++) {
        func(a[i]);
    }
}

int main(){
    
    int a[5] = {1,2,3,4,5};
    foreach_5(a, 5, add5);
    
    foreach_5(a, 5, print);
    
    cout << endl;
    Add add(35);
//    for_each(a, 5, Add(12));
    
    int x = 100;
    for_each1(a, 5, [&x](int &r){
        x = 200;
        r += 120;
    });
    
    foreach_5(a, 5, print);
    
    return 0;
}
```
