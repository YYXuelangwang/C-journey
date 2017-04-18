```
//
//  main.cpp
//  baseC++
//
//  Created by hundred wang on 17/3/20.
//  Copyright © 2017年 hundred wang. All rights reserved.
//

#include <iostream>
using namespace std;

class Vec{
    int x;
    int y;
public:
    Vec(int x, int y){
        this->x = x;
        this->y = y;
    }
};

Vec getPosition(){
    Vec v(100, 200);
    return v;
}

//void fa(int &a){
//    a ++;
//}

void fa(const int &r){
    // r++;
}

void fb(int *b){
    b ++;
}

void fc(int c){
    c++;
}

void swap(int *a, int *b){
    int c = *a;
    *a = *b;
    *b = c;
}

void swap(int &a, int &b){
//    int c = a;
//    a = b;
//    b = c;
    
    //不使用第三个变量的情况，交换两个数的值
    
    //方法一
    a = a + b;
    b = a - b;
    a = a - b;
    
    //方法二
//    a = a ^ b;
//    b = a ^ b;
//    a = a ^ b;
}

int & fd(int &r){
    r++;
    return r;
}

int& fe(){
    int x = 100;
    //永远不要返回一个局部变量的引用
    return x;
}

//这两个方法相比，ffa比ffb好，因为ffb多创建了一个变量
int ffa(const int &r){
    return r + 200;
}

int ffb(const int x){
    return x + 200;
}

int main(int argc, const char * argv[]) {
    // insert code here...
    std::cout << "Hello, World!\n";
    
    int a = 12, b = 20;
    cout << a << b << endl; // 输出12 20
    swap(a, b);
    cout << a << b << endl; // 输出20 12
    
    int x = 200;
    int &r2 = fd(x);
    cout << r2 << endl;   //输出201； x也等于201
    
    int &r4 = fe();
    //回收可能慢于调用，但这样做一定是错的
    cout << r4 << endl;
    
    x = 3;
    int r3 = fd(x);
    r3++;
    cout << "r3=" << &r3 << endl; //输出r3=0x7fff5fbff6ec
    cout << "x=" << &x << endl;   //输出x=0x7fff5fbff704
    
    fa(x);
    //如果要引用常量只能使用常引用
    const int &r = 300;
    
    fa(300);
    const int y = 300;
    fa(y);
    
    
    
    return 0;
}

```
