# cppcodetest


# c++ inheritance
``` cpp
// Online C++ compiler to run C++ program online
#include <iostream>
class Par
{
    private:
        int x;
        int y;
    public: 
        Par(int a, int b)
        {   x =a;
            y =b;
            std::cout << "parent class constructor called"<< std::endl;
            // if std:: not included then it will return cout not declared.
        }
    void display ()
    {
        std::cout<< x << "  "<< "thangaraj"  << std::endl;
        std::cout<< y << "  "<< "thangaraj"  << std::endl;
         
    }
};
class Cld : public Par
{
    public:
        Cld(int a, int b) : Par(a, b) {}
        void display()
        {
            std::cout<< "this thangaraj called "<< std::endl;
             //Par::display();      // if required display of parent class
        }
};

int main() {
    Cld cobj(10,20);  // cpp is objected created, addition the parent constructor also called.
    cobj.display();   // Child Class method called
    return 0;
}

```
# SIngleton 
``` cpp
// Online C++ compiler to run C++ program online
#include <iostream>
class Singleton
{
    private:
    static Singleton* instance ;
    //static std::once_flag initFlag;
        Singleton ()
        {
            std::cout<< "singleton object called" << std::endl;
        }
    public: 
static Singleton* getInstance()
    {
        instance = new Singleton();
        return instance ;
    }
    void callmethod();
};

void Singleton :: callmethod()
{
    std::cout << "Sigleton instance called" << std::endl;
}
// Initialize static members
Singleton* Singleton::instance = nullptr;
//std::once_flag Singleton::initFlag

int main() {
    Singleton * newinstance1 = Singleton::getInstance();
    newinstance1->callmethod();
    Singleton * newinstance2 = Singleton::getInstance();
    if (newinstance2 == newinstance1)
    {
        std::cout << "both object is same";
    }
   // Singleton Singleton1;
                       //if we are createing object then return error of
                        //: 'Singleton::Singleton()' is private within this context ;; 37 |     Singleton Singleton1;
   // Singleton1.callmethod();
    return 0;
}
```
