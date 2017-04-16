
命名空间
```
#include <cstdio>

//输入输出流
#include <iostream>

//命名空间，如果有冲突，还是得加上
using namespace std;

float const m = 12.0;

int main(int argc, const char * argv[]) {
    // insert code here...
//    int d;
//    std::cin >>d;
//    std::cout << "Hello, World!\n"<<d<< 'a' << "helloo,ageo";
    
    //std:: 名字空间 （命名空间）
    cout.operator<<(123455);
    
    int x, y, z;
//    std:: cin >> x;
//    std:: cout << "x = " << x <<std::endl;
    
    cin >> x >> y >> z;
    cout << "point(x,y,z) = (" << x << ", " << y << ", " << z << ")" << endl;
    
    cin >> x;
    cout << "你今天能将第一天的内容掌握嘛" << "这个时可以的，你得相信我，完全相信哦" << endl;
    
    //C++所有内容都在命名空间中， 只不过我们自己没有写命名空间， C++会把这
    //些内容放在一个没有名字的命名空间中
    cin >> ::m;
    cout << "这是一个全局变量" << :: m << endl;
    return 0;
}

```
```

#include <iostream>

//命名空间的常见用法
namespace hero {
    const char *name = "英雄";
    void show() {
        std:: cout << "英雄出现" << std:: endl;
    }
}

namespace enemy {
    const char *name = "敌人";
    void show() {
        std:: cout << "敌人出现" << std:: endl;
    }
}

//全局区
const char * name = "NPC";

const float m = 12.0;

int main(){
    
//    std::cout << hero::name << std::endl;
//    std::cout << enemy::name << std::endl;
//    std::cout << name << std::endl;
    
    //使用声明
//    using std::cout;
//    using std::endl;
//    using std::string;
//    
//    cout << "1234" << endl;
//    
//    string str = "123";
    
    //使用指令
//    using namespace std;
//    
//        cout << "1234" << endl;
//    
//        string str = "123";
    
    using namespace std;
    using namespace hero;
    
    cout <<hero:: name << endl;
    show();
    
    //如果存在着歧义还是需要加上命名
    using namespace enemy;
    cout <<enemy:: name << endl;
    enemy:: show();
    
    //C++所有内容都在命名空间中， 只不过我们自己没有写命名空间， C++会把这
    //些内容放在一个没有名字的命名空间中
    cout << ::name << endl;
    
    cout << "这是一个全局变量m" << ::m << endl;
    return 0;
}

```
