
```

class CCDirector{
    static CCDirector *m_director;
    CCDirector(){};
    CCDirector(const CCDirector& dir){};
public:
    static CCDirector* sharedDirector(){
        if (NULL == m_director) {
            m_director = new CCDirector;
        }
        return m_director;
    }
};

//给静态变量开辟内存空间
CCDirector* CCDirector::m_director = NULL;



//方法一
class UserInfo{
    static UserInfo * m_userInfo;
    UserInfo(){}
    UserInfo(const UserInfo &userInfo){}
public:
    static UserInfo* sharedUserInfo(){
        if (NULL == m_userInfo) {
            m_userInfo = new UserInfo;
        }
        return m_userInfo;
    }
    
};


UserInfo* UserInfo::m_userInfo = NULL;


//方法二
class Test;
static  Test *singleton = NULL;
class Test{
    Test(){}
    Test(const Test &t){}
public:
    static Test* sharedTest(){
        if (NULL == singleton) {
            singleton = new Test;
        }
        return singleton;
    }
};

int main(){
    
    CCDirector *dir = CCDirector::sharedDirector();
    CCDirector *dir2 = CCDirector::sharedDirector();
    
    cout << "dir = " << dir << endl;
    cout << "dir2 = " << dir2 << endl;
    
    UserInfo *us1 = UserInfo::sharedUserInfo();
    UserInfo &us2 = *us1;
    
//    CCDirector d = *dir2;
//    cout << "d =  " << &d << endl;
//    cout << "dir2 = " << dir2 << endl;
    
    return 0;
}
```
