
```
class Employee {
    string name;
    int age;
    
    mutable int x;
public:
    Employee(string name, int age) :
    name(name), age(age){}
    void setName(string name){
        this->name = name;
    }
    string getName() const{
        return name;
    }
//    代表多是返回的是const void
//    const void show()
    //const 用在 函数中 不能修改成员变量
    void show() const {
        //普通成员变量不能在const函数中修改，如果要修改投个成员变量，该变量必须
//        被mutable修饰才可以
        x = 100;
        
//        this->name = "sdf";
        cout << "" << name << ", " << age << endl;
    }
    
    //const 函数 与 非 const 函数可以形成方法重载的关系
    void show(){
        
        //        this->name = "sdf";
        cout << "" << name << ", " << age << endl;
    }
};

class Animal {
    string name;
    string kind;
    int age;
    mutable bool largeOrSmall;
public:
    Animal(string name, string kind, int age) :
    name(name), kind(kind), age(age) {}
    void setName(string name){
        this->name = name;
    }
    string getName() const {
        return name;
    }
    
    void feek() const {
        largeOrSmall = true;
        cout << name << ", " << age << "jiaole " << endl;
    }
    
    void feek() {
//        name = "eg";
//        age = 14;
        cout << name << ". " << age << endl;
    }
    
    Animal& growUp(){
        age++;
        return *this;
    }
};




int main(int argc, const char * argv[]) {
    
    Employee em("zhangsan", 30);
    em.show();
    em.setName("ioyo");
    em.getName();
    
    const Employee em2("lisi", 20);
//    const 对象只能调用 const类型的成员函数
    em2.show();
    em2.getName();
//    em2.setName("io");
    
    Animal A("ageo", "dog", 12);
    A.feek();
    
    const Animal B("gege", "cat", 3);
    B.feek();
    
    A.growUp().growUp().growUp().growUp();
    A.feek();
    
    return 0;
}

```
