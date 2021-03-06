#
##Introduction
<br>
**Discordance is a language that compiles into C++.** Despite it's insistence on importing code via Copy Pasting, C++ is an wickedly fast and sometimes
even readable language. Discordance attempts to highlight the good parts of C++ and paper over the bad parts.

Discordance is just C++. It compiles into C++ source with no performance penalties, and the header is generated for you. 
C++ constructs can be used in discordance, and vice versa. 
##Overview
**main.dis**
```c++
#include <iostream>
using namespace std;
//dynamic function
@ return_any(@ any):
  return any;
//Functions can be declared with python syntax
int main():
    //as can classes
    class apple:
        string color="green";
        //variables declared as type @ can take on any primitive value, including std::string
        @ weight="2kg";
        apple():
            weight=2;
            weight=2.0+weight;
            //use if just like a normal variable
            cout<<weight;
    //easy declaration of vectors
    int numbers[10?][?]={     //[num?] will declare a vector, [?] a deque
    {1,2,3,4,5,6,7,8,9,10},
    {-1,-2,-3,-4,-5,-6,-7,-8,-9,-10}
    };
    //python-style for statements
    for row in numbers:
        for column in row:
            cout<<column<<endl;
    //arrays of unlike things
    @ bucket[?]={2.0, "chocolate milk", "15789"};
    for item in bucket:
        cout<<item<<endl;
    //array slicing
    auto small_bucket=bucket[1:2];
    for item in small_bucket:
        cout<<item<<endl;
    //dynamic functions
    cout<<return_any(2.0)<<endl;
    cout<<return_any(5)<<endl;
    cout<<return_any("hello")<<endl;
    return 0;
```
**main.h**
```c++
// discordance.h
//
#ifndef LZZ_discordance_h
#define LZZ_discordance_h
#include "__Discordance.h"
using namespace discordance;
using discordance::deque; using discordance::vector; using discordance::var;
#include <iostream>
#define LZZ_INLINE inline
discordance::var return_any (discordance::var any);
int main ();
#undef LZZ_INLINE
#endif
```
**main.cpp**
```c++
// discordance.cpp
//

#include "discordance.h"
#include "__Discordance.h"
using namespace discordance;
using discordance::deque; using discordance::vector; using discordance::var;
#define LZZ_INLINE inline
using namespace std;
discordance::var return_any (discordance::var any)
                                                   {
  return any;
}
int main ()
          {
    class apple{
        string color="green";
        discordance::var  weight="2kg";
        apple(){
            weight=2;
            weight=2.0+weight;
            cout<<weight;
        }
    };
    discordance::deque<discordance::vector<int >> numbers={
    {1,2,3,4,5,6,7,8,9,10},
    {-1,-2,-3,-4,-5,-6,-7,-8,-9,-10}
    };
    for ( auto row : numbers ){
        for ( auto column : row ){
            cout<<column<<endl;
        }
    }
    discordance::deque<discordance::var  > bucket={2.0, "chocolate milk", "15789"};
    for ( auto item : bucket ){
        cout<<item<<endl;
    }
    auto small_bucket=bucket.slice(1,2);
    for ( auto item : small_bucket ){
        cout<<item<<endl;
    }
    cout<<return_any(2.0)<<endl;
    cout<<return_any(5)<<endl;
    cout<<return_any("hello")<<endl;
    return 0;
}
#undef LZZ_INLINE
```
##Installation
Currently, Discordance is only available on Windows. 

The transpiler requires g++. Make sure you have [Chocolatey](https://chocolatey.org/install) installed.
You can then install g++ through [Cygwin](https://www.cygwin.com).  
```bash
choco install cygwin
```
Download either the CLI or the GUI binary from my release [page](https://github.com/NeverLucky123/Discordant_Transpiler/releases). If you downloaded the CLI,
add it to PATH.
##Usage
###Command Line
Once `Transpiler.exe` is added to PATH, you should have access to the `Transpile` command in terminal. It takes two positional
arguements and 2 optional arguements.

| Option     | Expects                             | Description                                                                                                                                                                               |
|------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Position 1 | Either `transpile`,`make`, or `run` |  The action for the Transpiler to perform.  `transpile` generates C++ source files, `make` transpiles   and creates the executable, `run` does all of the above and runs the executable.  |
| Position 2 | File Path                           | The main input file. This should contain `main()`.                                                                                                                                        |
| -i         | Dir Path                            |  Include directories to search for header and source files in your project. Defaults to the directory of previous option.                                                                 |
| -o         | File Path                           |  The Path to the created executable. Defaults to File Path given + `.exe`.                                                                                                                |

####Examples
+ Generates C++ source files for `testfile.dis`  
   `transpiler transpile C:\Users\henry\PycharmProjects\Discordant_Transpiler\venv\Discordance\Include\testfile.dis`
+ Runs `testfile.dis`  
   `transpiler run C:\Users\henry\PycharmProjects\Discordant_Transpiler\venv\Discordance\Include\testfile.dis`
###GUI
No setup is needed. Just click on the executable. The GUI application will create two pickle files in your
application data directory under `.discordance/`. They take up minimal space and contains your applications settings.

**All your inputs are automatically saved, even past closing the app.**

+ `Input File` is the main file you want to transpile, make or run.

+ `Std Out` redirects any errors or program outputs.

+ `Configure->Includes` to search in more directories for header files.

+ `Configure->Cache` to clear the project cache. This is equivalent to a `--clean` build for all projects.

+ Use the toggle to select the action for the Transpiler to perform. `transpile` generates C++ source files, `make` transpiles and creates the executable, `run` does all of the above and runs the executable.


##Language Reference
This reference shows source Discordance above output C++. 
Familiarity with C++ and its STL is assumed. 
###Dynamic Typing
Variables declared with the @ keyword can be assigned any single primitive value.
This includes ints, floats, char[], and std::string and excludes pointers, references, or classes.
```c++
@ weight="2kg";
weight=2;
weight=2.0+weight;
```
```c++
discordance::var weight="2kg";
weight=2;
weight=2.0+weight;
```
Dynamic variables are implemented as a class in C++. This means they are around 60 times slower than their primitive counterpart,
or about as fast as a python primitive, mostly due to constructor overhead. They also cannot be passed into templated or overloaded functions,
as they cannot resolve their own type. 
###Dynamic Arrays
Dynamic arrays are declared with [?] or [**SOME_NUMBER**?]. If a number is given, then a `vector` is created, otherwise a `deque` is created. 
This is because if space in `vector` is not reserved, performance suffers immensely. 
```c++
std::string desserts[?];
desserts={"apple pie", "chocolate cake", "waffles", "cupcakes"};
std::string colors[5?];
colors={"green", "blue", "magenta", "teal", "crimson"};
```
```c++
discordance::deque<std::string > desserts(0);
desserts={"apple pie", "chocolate cake", "waffles", "cupcakes"};
discordance::vector<std::string > colors(5);
colors={"green", "blue", "magenta", "teal", "crimson"};
```
Dynamic arrays compile into `discordance::deque` or `discordance::vector`, which behave in the same way as their `std` counterparts except 
that they can be implicitly casted into `discordance::vector` of different types.
```c++
colors=desserts;
double calorie_count[?]={100.0, 350.3, 70.2, 50.2};
int precise_count[?]=calorie_count;
```
```c++
colors=desserts;
discordance::deque<double > calorie_count={100.0, 350.3, 70.2, 50.2};
discordance::deque<int > precise_count=calorie_count;
```
Multidimensional dynamic arrays can be declared as below. As many dimensions as you want.
```c++
int numbers[10?][?]={
    {1,2,3,4,5,6,7,8,9,10},
    {-1,-2,-3,-4,-5,-6,-7,-8,-9,-10}
};
```
```c++
discordance::deque<discordance::vector<int >> numbers={
    {1,2,3,4,5,6,7,8,9,10},
    {-1,-2,-3,-4,-5,-6,-7,-8,-9,-10}
};
```
Dynamic arrays can be created with the @ keyword, which gives them the ability to hold unlike primitive values.
```c++
@ bucket[?]={2.0, "chocolate milk", "15789"};
```
```c++
discordance::deque<discordance::var  > bucket={2.0, "chocolate milk", "15789"};
```
###Array Slicing
Arrays can also be sliced like so.
```c++
auto small_bucket=bucket[1:2];
```
```c++
auto small_bucket=bucket.slice(1,2);
```
###If, Else, Else If, While Statements
Statements can be declared with a Python-esque syntax.
```c++
if bucket[0]==2.0:
    cout<<bucket[0]<<endl;
else if bucket[1]=="chocolate milk":
    cout<<bucket[1]<<endl;
else:
    cout<<bucket[2]<<endl;
while true:
    cout<<"True"<<endl;
```
```c++
if ( bucket[0]==2.0 ){
    cout<<bucket[0]<<endl;
}
else if ( bucket[1]=="chocolate milk" ){
    cout<<bucket[1]<<endl;
}
else{
    cout<<bucket[2]<<endl;
}
while ( true ){
    cout<<"True"<<endl;
}
```
###For loops (Requires C++11)
For loops follow the same gist. If you don't specify a type for the loop variable, it'll be assumed to be auto.
```c++
int numbers[10?][?]={     
    {1,2,3,4,5,6,7,8,9,10},
    {-1,-2,-3,-4,-5,-6,-7,-8,-9,-10}
};
for row in numbers:
    for column in row:
        cout<<column<<endl;
```
```c++
discordance::deque<discordance::vector<int >> numbers={
    {1,2,3,4,5,6,7,8,9,10},
    {-1,-2,-3,-4,-5,-6,-7,-8,-9,-10}
};
for ( auto row : numbers ){
    for ( auto column : row ){
        cout<<column<<endl;
    }
}
```
You can specify whether to access by value or reference.
```c++
for &item in bucket:
    static int i;
    i++;
    cout<<item<<endl;
```
```c++
for ( auto &item : bucket ){
    static int i;
    i++;
    cout<<item<<endl;
}
```
Alternatively, you can manually specify the type of variable to cast to,
though this is not normally considered good practice. Refer to C++ standards.
```c++
for row in numbers:
    for int column in row:
        cout<<column<<endl;

```
```c++
for ( auto row : numbers ){
    for ( int column : row ){
        cout<<column<<endl;
    }
}
```
###Class and Function Declaration
Classes and functions can be declared much the same way.
```c++
class apple:
        string color="green";
        @ weight="2kg";
        apple():
            weight=2;
            weight=2.0+weight;
            cout<<weight;
```
```c++
class apple{
        string color="green";
        discordance::var weight="2kg";
        apple(){
            weight=2;
            weight=2.0+weight;
            cout<<weight;
        }
};
```
###Automatic Header Generation
Discordance is a header-less language. There is no need to write your own header files as one will be automatically generated for you.
Write your .dis file like its your source file. 
   
**a1.dis**
```c++
#include "a2.h"
void a1(){
    std::cout<<"a1"<<std::endl;
}
```
**a1.h**
```c++
#ifndef LZZ_a1_h
#define LZZ_a1_h
#include "__Discordance.h"
using namespace discordance;
using discordance::deque; using discordance::vector; using discordance::var;
#include "a2.h"
#define LZZ_INLINE inline
void a1 ();
#undef LZZ_INLINE
#endif
```
**a1.cpp**
```c++
#include "a1.h"
#include "__Discordance.h"
using namespace discordance;
using discordance::deque; using discordance::vector; using discordance::var;
#define LZZ_INLINE inline
void a1 ()
         {
    std::cout<<"a1"<<std::endl;
}
#undef LZZ_INLINE
```
###C++ Compatibility
Discordance is a superset of C++. All C++ is valid code. The only exception is because of automatic header generation and
dependency detection as part of the compilation process, `forward declaration` is not supported. 
###\#Include 
You can include `.h` files and `.dis` in both `.dis`, `.cpp` or `.cxx` files. Other extensions are currently not supported
and will cause compilation to fail. When `.dis` is included, the transpiler creates its `.h` and `.cpp` counterparts and
includes the created `.h` instead.

Basically, it works how you expect to. See compilation for additional details. 
##Build System
Discordance uses a build system that is capable of inferring all dependencies from your main file and recompiling only when
files or their dependencies have changed. Essentially, its GNU make with none of the setup.

Discordance does this by recursively parsing `#include` statements and then searching for those files in the given directories.
This means, however, that two files with the same name cannot exist in the given directories, as Discordance would not be able
to distinguish them. Discordance then checks if the file has changed or if one of its dependencies has changed to figure if to 
recompile it. It does this by checking the file modification time against the last time the executable of that name was compiled.
All executables that have ever been compiled and their last compile timestamp is kept in a cache file. 
###Things to Know
+ No makefiles required
+ Do not use forward declaration
+ Do not use absolute file paths in `#include`
+ Only changed files are compiled to lower compile time
+ Clear the cache if you want to rebuild all files.
+ .o files are kept in `.obj/` in their source directory.