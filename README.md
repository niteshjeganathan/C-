# CPP
## Basic C++ Syntax and Semantics
### Main()
> It is the main application routine. C++ is made up of classes and objects, but contains atleast one C-like function, called the main(). It's special feature is that, it has an implicit return 0 statement, since most operating systems interpret it as "program completed successfully".
```c++
#include <iostream>
using namespace std;

int main() {
    //main routine
    //return 0; Explicit call is suggested, but not required. 
}
```

### Default Parameters
> A function can have default parameters, when the caller doesn't supply a value. Default parameters should be declared in the function declaration, and should be omitted in the function definition.
```c++
#include <iostream>
#include "Car.h" //Will contain the function declaration with the default parameters declaration
// void f(int value = 5);
using namespace std;

void f(int value) {
    return value + 5;
}
```

### Automatic Objects (Stack Objects)
> When the object goes out of scope, the destructor is automatically called. The object is destroyed automatically, there is no need for explicit deallocation. These objects are created on the stack, and when it goes out of scope, the stack is "unwound".
```c++
#include <iostream>
using namespace std;

// When a function is called, program will store current track of execution context,
// like parameters, variables, return address(after the function is exited)
// This context is stored in activation record, or a stack frame
void fn() // When a function is called, a new stack frame is created on top of the call stack. 
{
    int a = 5; // Variable pushed in stack
    cout << a << endl; // Variable accessed from the stack
} // Stack is "unwound", and current context is changed back to the return address
```

### Dynamic Objects (Heap Objects)
> It uses the keyword "new". Memory manangement is left to the programmer. So a manual delete() call is required when the object is to be destroyed, or it will lead to memory leaks. Use of smart pointers, takes care of the memory management problems.
```c++
#include <iostream>
#include "Car.h"
using namespace std;

void fn()
{
    Car *carPtr = new Car(); //Dynamic Allocation of Memory in Heap
    carPtr->startEngine();
    carPtr->honk();
    delete (carPtr); //Manual Deallocation of Memory
    //If omitted, leads to memory leaks. 
}
```


