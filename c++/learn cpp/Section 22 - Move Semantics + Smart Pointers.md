22.1:

- Easy to forget a pointer delete through some type of early return.  
- Pointer variables have no inherent mechanism to clean up after themselves.  
- If you have a class, whatever is in the destructor gets automatically called (so you could deallocate in the destructor).   
- Smart pointer: composition class that manages dynamically allocated memory, ensuring memory gets deleted and then deleted when smart pointer goes out of scope.   
- No copy constructor or assignment operator, we do shallow copies, and res2 and res1 are pointed at the same resource.   
- We can explicitly define and delete the copy constructor and assignment operator, so we don’t make copies being made.  
  - Prevents pass by value case, but this would prevent returning something by value.   
- Could also make deep copies, but requires a lot of compute.  
- Move semantics:   
  - Copy constructor and assignment operator copy the pointer instead of move ownership of the pointer.  
- Deleting a nullptr does nothing.  
- Std::auto\_ptr, tried this, this has problems when passing something by value to a function because it just deletes the main thing.   
  - Deletes contents using no-array delete, and prevents passing of dynamic array.   
  - Doesn’t play nice with  other classes in the standard library, since library classes assume that when a copy happens, a move doesn’t happen.  
- Why issue?  
  - No way to differentiate copy semantics from move semantics. 

22.2:d

- Lvalue reference: reference that only binds to a modifiable L value.  
- Lalue references to const can be modified with modifiable and nonmodifiable l values and r values alike.   
  - Allows us to pass in any type of argument without making a copy of the argument.   
- R-value reference, only intended to be initialized with an r value (used with a double ampersand).   
- R value references: extent lifespan of object initialized with, non-const r values allow modification r-value.  
- You can overload parameters based on rvalue references and lvalue references.   
- R-value reference variables are lvalue.  
  - Type and value category are independent.  
- Can’t return an rvalue reference (goes out of scope).

22.3;

- Copy constructors: initialize a class by making a copy of an object of the same class.   
- Copy assignment; copy a class object to another one.  
- Compiler by default provides constructor and operator if one is not explicitly provided, but they do shallow copies.   
- Inefficient with deep copy creation.  
- Move constructor and move semantics.   
  - Move constructor and move assignment: moves ownership of an object to another   
  - These take non-const rvalue reference parameters.   
- How do these work?  
  - Steal source object’s resource, shallow copying source pointer into implicit object, then set to null.  
- Use noexcept.   
- The move constructor and move assignment called when functions have been defined, and the argument for construction is an r value.   
- Copy constructor and copy assignment used when the argument is an l value.   
- Implicit move constructor and move assignment operator created when no user-declared copy constructors or copy assignment operators, no user move constructors or assignment operators, no user declared destructor.  
  - Implicit constructor copies pointers not move.  
- Thus reasoning:  
  - If doing initialization with an L value, copy it.  
  - But if we know that we have an r value initliaization, that’s a temp, so rather than copy, just move resources.   
- Why set null pointer in r value object?  
  - When r value object goes out of scope, destructor gets called, if pointing same object, dangling pointer.   
- L values that are returned by value can be moved.  
- You can disable copying all together by deleting copy constructor and copy assignment.   
- Rule of five: if either the copy constructor, copy assignment, move constructor, move assignment, or destructor is defined or deleted, all of them should be.   
- Fleeting only move constructor and move assignment sounds good if you want copyable but not movable object, but prevents class not returnable in cases where copy elision doesn’t apply.

22.4:

- Std::move  
- We want to do move semantics with l values not r values.  
- Instead of doing copies, we can switch to move semantics  
- Std::move, converts an l value argument to an r value reference such that move semantics can be invoked.   
- Objects moved from should be reset to some default zero/state or should have whatever is the most convenient.   
- Std::move → only use persistent objects whose value you want to move, don’t ake assumptions about the value beyond that point. 

22.5:

- Std::unique\_ptr.  
- Smart pointer: manages a dynamically allocated resource and ensures it is cleaned up.  
- Std::unique\_ptr → completely owns the object it manages  
  - Must allocate this on the stack.  
- Must use move semantics when working with std::unique pointer.    
- Favor std::array, std::vector, std::string over smart pointer for managing a fixed array, dynamic array, or C-style string.   
- std::make\_unique(), constructors an object of the template type, initializes it with arguments passed into function.   
  - Prevents situations where a function call that throws an exception prevents deallocation of object.   
  - This is because std::make\_unique compartmentalizes creation of object and the creation of the pointer.  
- Can safely return std::unique pointer from a function by value.   
- Need to move the pointer into the temporary parameter.   
- Std::unique\_ptr:  
  - Can’t let meultiple objects manage the same resource.  
  - Don’t delete the underlying object. 

22.6:

- Std::shared\_ptr: multiple smart pointers co-own a resource.  
  - As long as one ptr is pointing to the resource, it will not be deallocated.  
- Cannot create shared pointers that are independent of each other because they don’t know of each other’s existence.  
  - Need to have one of them made by copy of another.   
- How does std::shared\_ptr work?  
  - Two intern pointers.  
  - One point at the resource being managed.   
  - The other point at a “control block”, dynamically allocated object tracking how many shared::ptr point at the resource.   
- When not using make\_shared(), allocation of the managed object and control block are allocated separately.  
  - However, with make\_shared(), this is optimized into a single allocation.   
- Explains why each pointer allocating its own control block is a problem (independent allocations of the control block result in only one pointer showing up).   
- Copy assignment: data in the control block can be updated.   
- A unique ptr can be converted into a shared ptr.   
- If the shared ptr is part of a dynamically allocated class thats never deleted, it will also not be deallocated.   
  - If any of the std::shared\_ptr managing a resource are nto deleted properly, the resource will not be deallocated

22.7:

-  If there’s a dependency of shared pointers, nothing gets deallocated because both shared pointers go out of scope seeing that another points to the shared resource. \`	  
- Circular reference: series o references where each object references the next, and the last object references to the first, resulting in a loop.   
- When shared pointers form a cycle, each object ends up keeping the next alive so no one can be deallocated.   
- Solution: use weak\_ptr (observe and access the esame object, but is not an owner).   
  - To use a weak\_ptr we must cast to shared ptr and then use std::shared\_ptr.   
- You can see the reference count and use that to see whether a weak pointer is pointing to something valid or not.