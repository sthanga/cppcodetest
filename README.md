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
