# cppcodetest

### https://github.com/AnthonyCalandra/modern-cpp-features?tab=readme-ov-file#concepts
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
#include <mutex>
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
        if (instance == nullptr)            // here the nullptr check required;
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
## Meson.build
``` meson.build
// explain details
project(
    'entity-manager',        # project name 
    'cpp',                   # Language used      
    default_options: [       # These are the default compiler and build settings
        'warning_level=3',   # Enable most compiler warnings (-Wall -Wextra and more).
        'werror=true',       # Treat all warnings as errors (-Werror).
        'cpp_std=c++20'      # Use C++20 standard for compiling (adds modern language features).
    ],
    license: 'Apache-2.0',   # This declares the project license — important for compliance and packaging (e.g., Debian/Yocto metadata generation).
    version: '0.1',          # This sets the project version
    meson_version: '>=0.58.0', # This ensures the Meson version being used is at least 0.58.0
)

add_project_arguments('-Wno-psabi', language: 'cpp')   # -Wpsabi is a GCC warning about "Public Symbol ABI changes".
                                                       # It warns you when the Application Binary Interface (ABI) of symbols (functions, structs, etc.)
                                                       # has changed due to compiler upgrades or architecture differences.
                                                       # It’s often triggered when using newer GCC versions or building for ARM/AArch64 platforms.

boost_args = [
    '-DBOOST_SYSTEM_NO_DEPRECATED',
    '-DBOOST_ERROR_CODE_HEADER_ONLY',
    '-DBOOST_NO_RTTI',
    '-DBOOST_NO_TYPEID',
    '-DBOOST_ALL_NO_LIB',
    '-DBOOST_ALLOW_DEPRECATED_HEADERS'
]
build_tests = get_option('tests')
cpp = meson.get_compiler('cpp')
boost = dependency('boost', required: false)
if not boost.found()
     subproject('boost', required: false)
     boost = declare_dependency(
         include_directories: 'subprojects/boost_1_71_0',
     )
     boost = boost.as_system('system')
endif
if get_option('fru-device')
    i2c = cpp.find_library('i2c')
endif

```
### namespace
``` cpp
#include <iostream>
#include <filesystem>
namespace fs = std::filesystem;    // alias is created here

int main() {
    fs::path p = "/tmp/test.txt";

    if (fs::exists(p)) {
        std::cout << "File exists: " << p << std::endl;
    } else {
        std::cout << "File not found." << std::endl;
    }
    return 0;
}

```
