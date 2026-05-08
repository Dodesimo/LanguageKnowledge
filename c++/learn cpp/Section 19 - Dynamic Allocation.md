19.1:

- Three basic types of memory allocation in C++.  
- First: static memory allocation: static and global variables.  
  - Memory allocated once when a program is run and persists throughout the life of the program.  
- Automatic memory allocation:  
  - Happens for function parameters and local variables.  
  - Memory allocated when the relevant block is entered and freed when the block is exited.  
- Commonalities: size of the variable/array must be known at compile time, memory allocation and dealocation happens automatically (when variable is instantiated/destroyed.  
- This doesn’t always work.  
  - Could lead to wasted memory, we don’t know what bits of memory are actually used.  
  - Most normal variables are allocated in a portion of memory called the stack (including fixed arrays)  
  - Dynamic memory: request memory from the operating system (comes from a pool of memory called the heap).  
- Use the new operator for a dynamic allocation.  
  - Returns a pointer containing the address of the memory.  
- Heap allocated objects are generally slower than accessing stack-allocated objects.  
  - Since compiler knows the address of stack allocated objects, can directly go there.  
  - Heap: get teh address, and then dereference to get the value.  
- Unlike static or automatic memory, program itself is responsible for requesting and disposing dynamically allocated memory.  
- Fleeting emrmoy: returns the memory back to the operating system.  
- No guarantees to what will happen to the contents of deallocated memory.  
- Pointer that is pointing to deallocated memory: dangling pointer.  
  - Dereferencing it.  
  - Deleting it as well.  
- Can have a situation where multiple pointers point at the same piece of dynamic memory.  
  - Ensure that all of them are taken up (nothing is left dangling).  
- Set deleted pointers to nulltpr unless they are going out of scope immediately  
- New operator can fail.  
- Use the std::nothrow keywords to ensure that the value is set to nullpointer when integer allocation fails.  
- Null pointers: tells us that no memory has been allocated to a particular pointer.  
  - Allows for conditional allocation of memory.  
- If pointer is null, nothing happens when you delete it.  
- However, if it is non-null, dynamically allocated memory is deleted.  
- Dynamical allocated memory stays allocated till its deallocated explicitly or until the program ends.   
  - But, the pointers that hold dynamically allocate dmemory addresses follow normal scoping rules.  
- So when the pointer is deleted, the program has lost the address of the memory.  
  - Memory leak: program loses the address of some bit of dynamically allocated memory before giving it back to the operating system.  
  - And then there’s no way for the program to give this memory back to the system.  
- Essentially a way to stop memory leaks is delete before allocating.

19.2:

- We can dynamically allocate arrays of variables.  
  - C-style arrays, because you might as well just use a std::vector for dynamically allocated std::arrays.  
- Use new type\[length\] to allocate an array.  
- Use delete\[\];  
  - How does delete know how much memory to delete?  
  - Array new keeps track of how much memory was allocated.  
- Dynamic array functions similar to a decayed fixed array (only has a pointer, no way to get length or size).  
  - But the programmer is responsible for deallocation.  
- C++ doesn’t have a built-in way to resize an array that has been already allocated. 

19.3:

- Destructor: special kind of class member function executed when we want to destroy an object of a class.  
- When an object goes out of scope normally or a dynamically allocated object is explicitly deleted with delete keyword, we call the class destructor.  
- For simple classes, a destructor isn’t needed because C++ automatically cleans up memory (simple class here is those that just initialize the values of normal member variables).  
- Use tilde for destructor types, can’t take arguments or return types.  
- Class can only have a single destructor.   
  - Don’t explicitly call a destructor, but a destructor can safely call other member functions since the object isnt destroyed until destructro executes.  
- Destructor called when object goes out of scope.  
- When initializing based on length, use parenthesis initialization.  
- RAII: programming technique where resource use is tied to lifetime of objects with automatic duration  
  - Resource acquisition is initialization.  
  - So a resource is acquired in an object’s constructor.  
  - That resource can thus be used while the object is alive.  
  - The resource is then released in the destructor.  
  - This prevents resource leaks as all resource-holding objects are cleaned up automatically.  
- std::exit() → we don’t call any destructors.

19.4:

- Pointer to pointers: exactly what it sounds like (double pointer)  
- Cannot directly set a pointer to a pointer directly to a value.  
- This is because & requires an lvalue, but \&value is an rvalue.  
- Where are pointers to pointers used?  
  - Dynamically allocate an array of pointers: int\*\* array {new int\*\[10\]}.  
- Making a two dimensional array.  
  - The best way is to request an array of pointers, then go through an initalize each pointer to an array.  
- When we delete, we need to go delete everything in the inside and then the outermost.

19.5:

- Void pointer: special type of pointer that can be poitned at objects of any data type.  
- Declared like a normal pointer, using the void keyword.  
- But because it points at objects of any data type, dereferencing it is illegal.  
- Void pointer must first be cast to another pointer type before we can do any dereferencing.  
- Void pointer can be set to a null value.  
- Because a void pointer doesn’t know what type of object its pointing to, fleeting it is undefined.  
  - Static cast to the right type.  
- Cannot do pointer arithmetic on a void pointer (this is because you need to know the type of of your object to do increment/decrement on the pointer appropriately).  
- 