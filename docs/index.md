This reference shows source Discordance above output C++. 
Familiarity with C++ and its STL is assumed. 

##@ Variables
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
##Dynamic Arrays
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
##Array Slicing
Arrays can also be sliced like so.
```c++
auto small_bucket=bucket[1:2];
```
```c++
auto small_bucket=bucket.slice(1,2);
```
##If, Else, Else If, While Statements
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
##For loops (Requires C++11)
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
##Class and Function Declaration
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
##Automatic Header Generation

##C++ Compatibility

##\#Include 