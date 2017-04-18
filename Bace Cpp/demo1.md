
NSObject.hpp
```
#ifndef NSObject_hpp
#define NSObject_hpp

#include <stdio.h>
#include <iostream>
using namespace std;

#define MY_PROPERTY(TYPE, M_NAME, FUNCTION_NAME)\
protected: TYPE M_NAME;\
void set##FUNCTION_NAME(TYPE count){\
    M_NAME = count;\
}\
TYPE get##FUNCTION_NAME(){\
    return M_NAME;\
}

#define MY_PROPERTY_READONLY(TYPE, M_NAME, FUNCTION_NAME)\
protected: TYPE M_NAME;\
public: TYPE get##FUNCTION_NAME() const{\
return M_NAME;\
}

class NSObject {
public:
    bool init();
    void retain();
    void release();
    void autorelease();
    
    //如果在.h文件中把声明和定义都实现出来了，这个
    //函数即使不加 inline 也是内联函数
    MY_PROPERTY_READONLY(int, m_retainCount, RetainCount);
    
    inline friend ostream& operator<<(ostream& o, NSObject *object){
        o << "NSObjec 被输出";
        if (object != nullptr) o << object->getRetainCount() << endl;
        else o << "NULL" << endl;
        return o;
    }
};

#endif /* NSObject_hpp */
```
***
NSObject.cpp
```
#include "NSObject.hpp"
bool NSObject::init(){
    return true;
}

void NSObject::retain(){
    m_retainCount++;
}

void NSObject::release(){
    if (m_retainCount > 0) m_retainCount--;
}

void NSObject::autorelease(){
    if (m_retainCount > 0) m_retainCount--;
}
```
***
NSMutableArray.hpp
```
#ifndef NSMutableArray_hpp
#define NSMutableArray_hpp

#include <stdio.h>
#include "NSObject.hpp"
#include <iostream>
using namespace std;

class NSMutableArray : public NSObject {
    MY_PROPERTY_READONLY(int, m_count, Count);
private:
    //数组的容量
    int m_capacity;
    
    //指向了一个放指针数组的首地址 eg： int a[] ; int *a;
    NSObject **m_objects;
    void expand();
public:
    //explicit 限制了构造方法 不可隐式类型转换
    explicit NSMutableArray(int capacity);
    static NSMutableArray* arrayWithCapacity(int capacity);
    bool initWithCapacity(int capacity);
    NSObject* objectAtIndex(int index) const;
    
    void addObject(NSObject* object);

    //从指定位置插入元素,这里参数object不能为const，不知道为什么
    void insertObjectAtIndex(int pos, NSObject* object);
    
    void replaceObjectAtIndex(int pos, NSObject* object);
    
    //删除尾部元素
    void removeLastObject();
    
    void removeObjectAtIndex(int pos);
    
    void removeObject(NSObject* object);
    
    //删除所有元素
    void removeAllObject();
    
    //查找指定元素位置
    int indexOfObject(NSObject* object);
    
    NSObject* operator[](int index);
    NSObject* operator[](char index){
        return objectAtIndex(index - 96);
    }
    NSObject* operator[](double index){
        return objectAtIndex(index + 0.5);
    }
    
    friend ostream& operator<<(ostream &o, const NSMutableArray* array);
};

#endif /* NSMutableArray_hpp */

```
***
NSMutableArray.cpp
```
#include "NSMutableArray.hpp"

#pragma mark - struct Method

NSMutableArray::NSMutableArray(int capacity) : m_capacity(capacity), m_count(){}

NSMutableArray* NSMutableArray::arrayWithCapacity(int capacity){
    NSMutableArray *array = new NSMutableArray(capacity);
    array->retain();
    if(array && array->initWithCapacity(capacity)){
        array->autorelease();
        return array;
    };
    array->release();
    delete array;
    return nullptr;
}


bool NSMutableArray::initWithCapacity(int capacity){
    if (!NSObject::init()) return false;
    m_count = 0;
    
    //int *a = new int a[10];
    m_objects = new NSObject*[capacity];
    m_capacity = capacity;
    return true;
}

#pragma mark - change Method
//扩容
void NSMutableArray::expand(){
    m_capacity *= 2;
    NSObject **temp = new NSObject*[m_capacity];
//    for (int i = 0; i < m_count; i++) {
//        temp[i] = m_objects[i];
//    }
    //内存拷贝参数：1目标 2源 3拷贝内存大小
    memcpy(temp, m_objects, m_count * sizeof(NSObject*));
    delete[] m_objects;
    m_objects = temp;
}



void NSMutableArray::addObject(NSObject *object){
    this->insertObjectAtIndex(m_count, object);
}

void NSMutableArray::insertObjectAtIndex(int pos, NSObject* object){
    if (pos < 0 || pos > m_count) return;
    if (m_count == m_capacity) this->expand();
    for (int i = m_count; i > pos; i--) {
        m_objects[i] = m_objects[i-1];
    }
    object->retain();
    m_objects[pos] = object;
    m_count++;
}

NSObject* NSMutableArray::objectAtIndex(int index) const{
    if (index < 0 || index >= m_count) return NULL;
    return m_objects[index];
}

void NSMutableArray::replaceObjectAtIndex(int pos, NSObject* object){
    if (pos < 0 || pos > m_count) return;
    m_objects[pos]->release();
    delete  m_objects[pos];
    m_objects[pos] = object;
    object->retain();
}

int NSMutableArray::indexOfObject(NSObject* object){
    for (int i = 0; i < m_count; i++) {
        if (object == m_objects[i]) return i;
    }
    return -1;
}

#pragma mark - remove Method

void NSMutableArray::removeObjectAtIndex(int pos){
    if (pos < 0 || pos >= m_count) return;
    m_objects[pos]->release();
    delete m_objects[pos];
    for (int i = pos; i < m_count-1; i++) {
        m_objects[i] = m_objects[i+1];
    }
    m_count--;
}

void NSMutableArray::removeObject(NSObject* object){
//    for (int i = 0; i < m_count; i++) {
//        if (object == m_objects[i]) {
//            removeObjectAtIndex(i);
//        }
//    }
    int pos = this->indexOfObject(object);
    if (pos != -1) removeObjectAtIndex(pos);
}

void NSMutableArray::removeAllObject(){
    while (m_count) {
        removeObjectAtIndex(0);
    }
}

void NSMutableArray::removeLastObject(){
    m_objects[m_count-1]->release();
    delete m_objects[m_count-1];
    m_count--;
}

#pragma mark - operator Method

ostream& operator<<(ostream &o, const NSMutableArray* array){
    for (int i = 0; i < array->getCount(); i++) {
        o << array->objectAtIndex(i) << endl;
    }
    return o;
}


NSObject* NSMutableArray::operator[](int index){
    return objectAtIndex(index);
}

```
***
UIView.hpp
```
#ifndef UIView_hpp
#define UIView_hpp

#include <stdio.h>
#include "NSObject.hpp"

class UIView : public NSObject{
public:
    static UIView* view();
    bool init();
};

#endif /* UIView_hpp */
```
***
UIView.cpp
```
#include "UIView.hpp"

UIView* UIView::view(){
    UIView *view = new UIView;
    view->retain();
    view->init();
    view->autorelease();
    return view;
}

bool UIView::init(){
    if (!NSObject::init()) {
        return false;
    }
    return true;
}

```
***
arrayTest.cpp
```
#include <iostream>
#include "NSObject.hpp"
#include "NSMutableArray.hpp"
#include "UIView.hpp"
using namespace std;



int main() {
    
    NSMutableArray *array = NSMutableArray::arrayWithCapacity(100);
    cout << array->getCount() << endl;
    UIView *v = UIView::view();
    cout << v->getRetainCount() << endl;
    array->addObject(v);
    cout << v->getRetainCount() << endl;
    UIView *v2 = UIView::view();
    array->addObject(v2);
    cout << v->getRetainCount() << endl;
    cout << v2->getRetainCount() << endl;
//    array->removeLastObject();
    cout << "v -> " << v->getRetainCount() << endl;
    cout << "v2 -> " << v2->getRetainCount() << endl;
    cout << array << "cout=" << array->getCount() << endl;
    
    UIView *v3 = UIView::view();
    array->removeObjectAtIndex(0);
    cout << array << "cout=" << array->getCount() << endl;
    
    NSMutableArray *array2 = NSMutableArray::arrayWithCapacity(15);
    
    
    cout << (*array)[3] << endl;
  
    //构造方法使用了explicit修饰词，限制使用隐式转换
//    NSMutableArray array3 = 200;
    
    return 0;
}
```
