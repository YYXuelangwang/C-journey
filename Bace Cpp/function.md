
```

//重载和重写
/*
 * 重载 方法名相同，参数列表不同
 * 重写 必须依赖于继承，子类重写（覆盖方法）父类方法
 */
 
//方法重载，方法名相同，参数列表不同（类型不同，个数不同，顺序不同），那么该两个函数就形成了重载的关系
int addExp(int heroExp, int addExp){
    return heroExp + addExp;
}

int addExp(int heroExp, double addExpPer){
    return heroExp * (1 + addExpPer);
}

//告诉编译器，该函数按C的方式处理，
extern "C"
int addExp(int heroExp, double addExpPer, int x){
    return heroExp * (1 + addExpPer);
}

//这里使用默认值
int addExp(double heroExp, double addExpPer = 2, int x = 0, int y = 20 , int z = 19){
    return heroExp * (1 + addExpPer) + x + y + z;
}

int main(){
    int heroExp = 1000;
    cout << addExp(heroExp, 100) << endl;
    cout << addExp(heroExp, 0.5) << endl;
    
    //这里使用的带默认值的函数
    cout << addExp(0) << endl;
    return 0;
}

```
cout
```
1100
1500
39
```

