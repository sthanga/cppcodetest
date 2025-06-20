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
## namespace, using
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
/*****************************************************************************
using phosphor::logging::commit;
using phosphor::logging::elog;
using phosphor::logging::entry;
using phosphor::logging::level;
using phosphor::logging::log;
🔹 What is using in C++?
The using directive or declaration tells the compiler:

“From now on, I’ll refer to this name without writing the full namespace path.”
**********************************************************************************/

```
### C++ dynamic memory allocation concept:

Yes, in C++, the `new` keyword is used to dynamically allocate memory for **any data type** — whether it's a **primitive** (like `int`, `float`), a **class**, a **struct**, or an **array**.

---

## ✅ General Syntax of `new`

```cpp
Type* ptr = new Type;
```

This dynamically allocates memory on the **heap**, and returns a pointer to the object.


## 🔹 1. **For Primitive Data Types**

```cpp
int* pInt = new int;        // Allocates memory for one int
*pInt = 42;

float* pFloat = new float(3.14); // Initializes float
```

> Don't forget to `delete pInt;` and `delete pFloat;` to avoid memory leaks.

---

## 🔹 2. **For Arrays**

```cpp
int* arr = new int[5];  // Allocates array of 5 ints
arr[0] = 1; arr[1] = 2; // ...

// Free the memory
delete[] arr;
```

Note: Arrays must be deallocated using `delete[]`.

## 🔹 3. **For Structs and Classes**

### Struct:

```cpp
struct Point {
    int x;
    int y;
};

Point* pt = new Point;         // No constructor here
pt->x = 10;
pt->y = 20;
```

### Class:

```cpp
class Person {
public:
    Person(std::string n) : name(n) {}
    std::string name;
};

Person* p = new Person("Alice");
std::cout << p->name << std::endl;
delete p;
```
## 🔹 4. **Using `new` with `auto`**

```cpp
auto ptr = new double(9.81);  // type is double*
```

## ❗ Important: Always Pair `new` with `delete`

If you use `new`, you **must** use `delete` (or `delete[]`) to free the memory:

```cpp
int* p = new int(10);
// ...
delete p;  // prevent memory leak
```

## 🔹 5. **Modern C++: Prefer `std::unique_ptr` or `std::shared_ptr`**

Instead of raw `new/delete`, use smart pointers to manage memory automatically:

```cpp
#include <memory>

auto ptr = std::make_unique<int>(100);      // C++14
auto shared = std::make_shared<int>(200);   // C++11

// No need to call delete
```

| Use Case           | Syntax Example                  |
| ------------------ | ------------------------------- |
| Primitive          | `int* p = new int(5);`          |
| Array              | `int* arr = new int[10];`       |
| Class/Struct       | `MyClass* obj = new MyClass();` |
| Delete             | `delete p;`, `delete[] arr;`    |
| Modern (preferred) | `std::make_unique<Type>(...)`   |

Let me know if you want an example managing dynamic objects in a loop or passing them between functions.

## vector , iterator, file open and json file parse;
```cpp
#include <iostream>
#include <fstream>    // file operation
#include <vector>     // vector
#include <nlohmann/json.hpp>  //jsonfile parse;
int main()
{
    std::vector<int> ciperlist;

    std::ifstream jsonFile("ciper.json");

    if (!jsonFile.is_open())
        std::cout<< "File Not openalbe" << std::endl;
    else
        std::cout<< "File opened" << std::endl;

    auto data = nlohmann::json::parse(jsonFile);

    ciperlist.push_back(0x00);
    std::string cipher = "cipher";
    for (const auto& list : data)
    {
        ciperlist.push_back(list.value(cipher, 0));
    }
    for (auto& ret : ciperlist)
        std::cout<< ret << std::endl;
/*******************another method iterate*****************
      for (int val : ciperlist)
      {
          std::cout << val << " ";
      }
      std::cout << std::endl;
**********************************************************/
}
/*
output
user:~/Videos/cpptestcode/ifstream$ ./ifstm 
File opened
0
12
13
14
15

for another method:
user@BLRTSL01069:~/Videos/cpptestcode/ifstream$ ./ifstm 
File opened
0 12 13 14 15 
*/
```
#### ciper.json
```json
[
  { "cipher": 12 },
  { "cipher": 13 },
  { "cipher": 14 },
  { "cipher": 15 }
]
```
## VECTOR 
``` cpp
#include <vector>
#include <iterator>

//Creating an empty vector
std::vector<int> v1;
std::vector<int> test = {1,2,3,5,6,7};     //vector<Type> Name:
//    for (auto &data : v1)
//        std::cout<<data<<std::endl;     // this way also print it.
std::vector<int>std::Iterator it;
for (it)

```
## vector full method try
``` cpp
// Online C++ compiler to run C++ program online
#include <iostream>
#include <vector>
#include <iterator>
#include <map>
#include <stack>
#include <queue>
#include <list>
#include <set>
#include <algorithm>

int main() {
    std::vector<int> v1;
    v1 ={ 1,2,3,4,5,6,7,8};
    for (auto &data : v1)
        std::cout<<data <<" ";
    std::cout<< "Vector initialized element" <<std::endl;
    /*"push_back()"  method is used to insert a element last*/
    v1.push_back(20);      // here the new element inserted
    for (auto &data : v1)
        std::cout<<data <<" ";
    std::cout<< "-- insert last 20" <<std::endl;
    
    /*"pop_back()"  method is used to remove last element */
    v1.pop_back();
    for (auto &data : v1)
        std::cout<<data <<" ";
    std::cout<< "-- removed last 20" <<std::endl;
    /*"size()" return the size of the vector */
    std::cout<< "vector size : " << v1.size() << std::endl;
    /*"size()" return the Max hold size */
    std::cout<< "Max hold size : " << v1.max_size()<< std::endl;
    v1.resize(9);
    std::cout<< "vector resize : " << v1.size() << std::endl;
        for (auto &data : v1)
        std::cout<<data <<" ";
    std::cout<< "-- resize of vector" <<std::endl;
    std::cout<<"-- vector check :" << v1.empty() <<std::endl;
    // return 0 if element loaded; or not zero  if empty;
    std::cout<<"operator[3] :"<<v1[3] << "-- at(5) :" << v1.at(5) <<std::endl;
    std::cout<<"front ():"<<v1.front() << "-- end ():" << *(--v1.end()) <<std::endl;
    // like rbegin(), rend(), cbegin, cend, crbegin, crend, emplace(), emplace_back(), assign(), capacity(), reserve(), shrink_to_fit(), data(), get_allocator, swap() method available.
    v1.insert(v1.begin() + 2, 32);
    for (auto &data : v1)
        std::cout<<data <<" ";
    std::cout<<"--insert 32 :"<<std::endl;
    v1.erase(v1.begin() + 2);
    for (auto &data : v1)
        std::cout<<data <<" ";
     std::cout<<"-- Erased 32:"<<std::endl;  
    v1.clear();
    for (auto &data : v1)
        std::cout<<data <<" ";
     std::cout<<"-- clear():"<<std::endl;  
    return 0;
}
/* outputs
1 2 3 4 5 6 7 8 Vector initialized element
1 2 3 4 5 6 7 8 20 -- insert last 20
1 2 3 4 5 6 7 8 -- removed last 20
vector size : 8
Max hold size : 2305843009213693951
vector resize : 9
1 2 3 4 5 6 7 8 0 -- resize of vector
-- vector check :0
operator[3] :4-- at(5) :6
front ():1-- end ():0
1 2 32 3 4 5 6 7 8 0 --insert 32 :
1 2 3 4 5 6 7 8 0 -- Erased 32:
-- clear():
*/

```
## Template Usage:
``` cpp
// C++ templates allow you to write generic, type-independent code.
// You can use templates to define functions or classes that work with any data type or even constant values.

#include <iostream>

template <typename T>    // template apply
T add( T a, T b)
{
    return a+b;
}
int main() {
    
   std::cout<< add (10, 20) << std::endl;
   std::cout<< add(43.33, 44.33) << std::endl;
   return 0;
}
/* output:
30
87.66
*/
```
### ✅ C++ Templates — Complete Beginner-Friendly Explanation

C++ templates allow you to write **generic**, **type-independent** code. You can use templates to define **functions** or **classes** that work with any data type or even constant values.

---

## 🔷 1. Function Template (Type-based)

### ✨ Example:

```cpp
template <typename T>
T add(T a, T b)
{
    return a + b;
}
```

### ✅ Usage:

```cpp
int result1 = add<int>(5, 3);         // 8
double result2 = add<double>(2.5, 4); // 6.5
```

* `T` is a placeholder for a data type (`int`, `double`, `float`, etc.)
* Compiler automatically replaces `T` based on usage.

---

## 🔷 2. Class Template

### ✨ Example:

```cpp
template <typename T>
class Box
{
public:
    T value;
    void set(T val) { value = val; }
    T get() { return value; }
};
```

### ✅ Usage:

```cpp
Box<int> intBox;
intBox.set(10);
std::cout << intBox.get();  // 10

Box<std::string> strBox;
strBox.set("hello");
std::cout << strBox.get();  // hello
```

---

## 🔷 3. Non-Type Template Parameter (like your case)

### ✨ Example:

```cpp
template <int N>
void printNTimes()
{
    for (int i = 0; i < N; ++i)
    {
        std::cout << "Hello\n";
    }
}
```

### ✅ Usage:

```cpp
printNTimes<3>(); // Prints "Hello" 3 times
```

* `N` is an integer constant known at **compile-time**.
* This is exactly what your `template <int family>` is doing.

---

## 🛠 Common Use Cases

| Template Type          | Used For                                                            |
| ---------------------- | ------------------------------------------------------------------- |
| `template<typename T>` | Generic containers, algorithms (e.g., `std::vector<T>`)             |
| `template<int N>`      | Compile-time values like array sizes, address families, loop counts |
| Class templates        | `std::vector`, `std::map`, `std::unique_ptr`, etc.                  |
| Function templates     | `std::swap`, `std::sort`, etc.                                      |

---

## 📌 Summary

| Concept                | Meaning                                                    |
| ---------------------- | ---------------------------------------------------------- |
| `template<typename T>` | T is a type (e.g., `int`, `double`)                        |
| `template<int N>`      | N is a compile-time constant (e.g., array size, IP family) |
| `template<class T>`    | Same as `typename` — just older syntax                     |

---

Would you like a full example using both class and function templates together?

