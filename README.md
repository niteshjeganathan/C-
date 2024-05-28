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

### Automatic Objects (Stack Objects)
> When the object goes out of scope, the destructor is automatically called. The object is destroyed automatically, there is no need for explicit deallocation. These objects are created on the stack, and when it goes out of scope, the stack is "unwound".

### Dynamic Objects (Heap Objects)
> It uses the keyword "new". Memory manangement is left to the programmer. So a manual delete() call is required when the object is to be destroyed, or it will lead to memory leaks. Use of smart pointers, takes care of the memory management problems.

