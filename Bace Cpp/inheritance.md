
```
class Parent{
public:
    int i1 = 100;
protected:
    int i2 = 200;
private:
    int i3 = 300;
};


class Child1 : public Parent{
public:
    void show(){
        i1 = 200;
        i2 = 300;
//        i3 = 100;
    }
};

class Child2 : protected Parent{
public:
    void show(){
        i1 = 200;
        i2 = 300;
//                i3 = 100;
    }
};

class Child3 : private Parent{
public:
    void show(){
        i1 = 200;
        i2 = 300;
        //                i3 = 100;
    }
};

class Child4 : public Child3{
public:
    void show(){
//        i1 = 200;
//        i2 = 300;
        //                i3 = 100;
    }
};

class MyRole{
public:
    //英雄使用
    void attack1() {}
    void attack2() {}
    void attack3() {}
    //敌人使用
    void attack4() {}
    void attack5() {}
    void attack6() {}
};

class Hero : private MyRole{
public:
    void hero_attack(){
        this->attack1();
    }
    void hero_attack1(){
        this->attack2();
    }
    void hero_attack2(){
        this->attack3();
    }
};

//OC 和 Java 中都没有私有继承，如何解决封装问题，
//这种情况我们可以试着不用继承而使用组成的方式解决

class Enemy{
    MyRole role;
public:
    void enemyAttack1(){
        role.attack4();
    }
};

int main(int argc, const char * argv[]) {
    
    Child1 c1;
    c1.i1 = 200;
//    c1.i2 = 100;
//    c1.i3 = 300;
    
    Child2 c2;
//    c2.i1 = 300;
    
    return 0;
}

```
***
inheritance2
```
/*
 1,开辟内存空间
 2，递归创建父类对象
 3，初始化成员变量， 如果是对象构建它
 4，调用构造方法
 */

class Enemy {
    const char* name;
public:
    Enemy(const char* name) : name(name){}
    Enemy(){}
    void attack(){
        cout << "攻击" << endl;
    }
    void idle(){
        cout << "待机" << endl;
    }
    const char* getName(){
        return name;
    }
};

class DemonArrow : public Enemy{
public:
    DemonArrow(const char* name) : Enemy(name){}
};

int main(){
    
    DemonArrow da("初级弓箭手");
    cout << da.getName() << endl;
    da.attack();
    da.idle();
    
    
    return 0;
}
```
