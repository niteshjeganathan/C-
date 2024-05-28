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

### Pass by Value 
> Objects are passed by value to a function, if it needs a local copy to work with. Changes made inside the scope of the function, will not reflect outside the scope of the function. The local object is deleted as it gets out of scope. 
```c++
#include <iostream>
#include "Bike.h"
using namespace std;

void fn(Bike bike)
{
    bike.startEngine(); //Changes here wont affect the bike outside the scope
}

int main() {
    Bike bike;
    fn(bike); 
    // No changes made to the bike here 
}
```

### Pass by Reference
> Objects are passed by reference to a function, when the function wants to retain ownership of the object even after the call returns to the caller. Since the object ownership is retained, the object isn't deleted when the function gets out of scope. 
```c++
#include <iostream>
#include "Bike.h"
using namespace std;

void fn(Bike& bike)
{
    bike.startEngine(); //Changes here will affect the bike outside the scope
}

int main() {
    Bike bike;
    fn(bike); 
    //Changes are made to the bike here 
}
```

### Pass by Pointer
> Objects are rarely passed by raw pointers. Usually they are passed using smart pointers. Objects are passed by smart pointers to a function, when the ownership of the object is to be transferred. Raw pointers are used when function needs to handle, NULL as a valid input. Use of smart pointers, leads to deletion of the object as the function gets out of scope, since the ownership has been transferred. But the use of raw pointers doesn't lead to deletion once the function gets out of scope.
```c++
#include <iostream>
#include <memory>
#include "Bike.h"
using namespace std;

typedef auto_ptr<Car> carPtr;

void fn(Car *car) // Use of raw pointers
{
    // function body
} // car doesn't get deleted here

void fn(carPtr car)
{
    // function body
} // car get's deleted here, since ownership has been transferred
```

### Stream Output
> Cout looks as if it is shifting stuff into the output stream object. Sometimes they are buffered, before sending it to the actual destination, for performance improvements. But this can be bypassed, using the flush() command.
```c++
#include <iostream>
using namespace std;

int main() {
    cout << "Hello World" << "\n" << flush; // Manual next line and flush
    cout << "Hello World" << endl; // Combines the next line followed by flush
    //Flushing I/O buffers can slow down the application
    //So should be avoided
}
```

### Stream Input
> Cin looks as if it is shifting stuff from the input stream object. There is no need to flush the stream, after a cout right before cin, since cout takes care of it.

### Variable Declaration
> In C style programming, the variables are conventionally created in the beginning of the scope. It promotes, readability, maintainability, scope clarity, and avoids uninitialised variables. In older compilers, it's more efficient when the variables are created in the beginning. Although, recent C compilers have been optimised for "mixed declarations and code". Yet the convention in C seems to remain the same.

> In C++ style programmming, it is advised to create variables as and when required, since some parts of the code can be avoided due to control flows and alternate variables can be created accordingly. This is a minor improvement over the C convention. Nothing to lose by following this newer convention, in some cases, we have something to gain. 

### Container Classes
> Most common use of templates are container classes. Container classes are used to create objects that hold other objects. Examples are, maps, vectors, linked lists and so on.

### Creating Header Files
> While creating header files, **header guards** or **include guards** are used to make sure, the header file is processed only once. They are preprocessor directives.
```c++
#ifndef Bike.h
#define Bike.h

// Contents of the bike header

#endif
```

### Constructor
> Special member function used to "construct" an object by initialising member data. They are not virtual, and have no return type. It allows an initialisation list for convenience. When a constuctor isn't defined, a default constructor that does nothing is implicitly defined by the compiler. When a parameterised constructor is already present, a default constructor is not implicitly defined by the compiler.
```c++
#include <iostream>
using namespace std;

class Bike {
private: 
    string manufacturer; 
    int horsepower; 
    string model;
public:
    Bike() = default; // Explicit DEclaration of a default constructor
    Bike(string m, int h, string mo) : manufacturer(m), horsepower(h), model(mo) {
        //Constructor body
    }
};

int main() {
    Bike bike = Bike("RE", 80, "Himalayan");
}
```

### Destructor
> Special member function used to "destruct" an object by cleaning up. It closes any files it opened, releases resources, unlocks semaphores and so on. Destructors can be virtual, making sure the child can implement its own destructor. Only one destructor can be defined for a class, and has no return type. If a class doesn't have a destructor, the compiler generates a destructor that does nothing. So when nothing is to be done, its best to omit the destructor and let the compiler generate one.
```c++
#include <iostream>
using namespace std;

class Bike {
public: 
    ~Bike() {
        //Releases any locks, resources and prevents memory leaks
    }
};
```

### Inheritance and Dynamic Binding 
> It enables extensibility. It allows to capture a is-a relationship or kind-of relationship (Car is-a vehicle, Dog is a kind-of Animal).

> Abstract Classes are incomplete classes, which are then completed by its derived classes. If properly designed, it enables extensibility by avoiding the ripple effect when new derived classes have been added.

> is-a conversion is the feature that allows the compiler to convert a derived class to it's base class. Dynamic binding is the feature that allows the compiler to convert back the base class to the appropriate derived class.

```c++
#include <iostream>
using namespace std;

class MediaPlayer {
private:
    string name; 
public: 
    virtual void setup() = 0;
};

class MP3Player : public MediaPlayer {
public: 
    void setup() {
        cout << "MP3 is setting up..." << endl;
    }
};

class BlueRayPlayer : public MediaPlayer {
public: 
    void setup() {
        cout << "Blue Ray is setting up... " << endl;
    }
};

void initPlayer(MediaPlayer& player) { //At the time of compilation, it doesn't know
// about the derived classes
    player.setup();
}

int main() {
    MP3Player m;
    BlueRayPlayer b;
    initPlayer(m); //is-a conversion at the parameter level
    initPlayer(b); //dynamic binding inside the body of the function
    //old code calling new code
}
```

