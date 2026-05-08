25.1:

- Derived pointers and references can be set to derived objects.  
- Can set base pointers and referenced to derived objects.  
  - But only base methods are seen through this.  
- Why use base classes?  
  - Keep an array of derived (because they all need to be the same type)

25.2:

- What is a virtual function?  
  - Resolves to the most derived version of a function for the actual type of an object being referenced or pointed to.  
- Derived function: a match if it has the same signature (name, parameter types, and const-ness) and returns the same type as the base version of the function.  
- Virtual function resolution only happens when a member function is called through a pointer or reference to a class type object.  
- Calling virtual member function directly on an object invokes the member function belonging to the same type of that object.  
- Compile time polymorphism: polymorphism resolved by the compiler, includes function overload resolution/template resolution.  
- Runtime polymorphism: polymorphism thats resolved at runtime.  
- So when we have a virtual function, program looks at all classes between animal and cat to see if there’s a more derived function.  
- If a function is marked as virtual, all matching overrides in derived classes are also implicitly considered virtual, even if they aren’t explicitly marked as such.  
- Should not call virtual functions from constructors or destructors.  
  - If a virtual function was getting called in the base constructor or destructor, none of the derived functions have been made yet.

25.3:

- A derived class virtual function is only considered an override if signature and return types match exactly.  
  - Different parameters as well as different const-ness, those functions are not considered overrides.  
- It might be hard to figure out what functions are overrides, so this should be used for all functions.  
  - Virtual keyword: virtual functions in a base class.  
  - Override specifier on override functions in derived classes.  
  - Const is listed first, override is listed next.  
- Final specifier is used to enforce lack of overrides.   
- Can apply final to a class to prevent overrides.  
- Covariant return types:  
  - If return type is a pointer or reference to some class, override functions can return a pointer or reference to a derived class.  
  - If a pointer from a base is used to call that function, a Pointer ot the base is returned (even if the function is virtual and returns a pointer to the derived class, it just gets upcasted).  
  - Used anytime the virtual member function returns a pointer or reference to the class containing the member function. 

25.4:

- Make destructors always virtual.  
  - If a derived class does memory allocation, and the destructor is not virtual, if the pointer is of the Base, deleting the pointer only calls the base destructor.  
- When you want a non-virtualized method to be called, use the scope resolution operator to select the right function.  
- If you want class to be inherited from, make the destructor virtual and public.  
- Else, market the class as final.

25.5:

- Function binding: process that determines what function definition is associated with a function call.  
- Dispatching: actually invoking a bound function.  
- Most function calls are direct calls, made to a non-member function or non-virtual member function.  
  - This is done at compile time and is thus static/early binding.  
  - Compiler or linker can generate machine language instructions at compile time to jump to that address.  
  - This is also applicable to overloaded functions and function templates.   
- Direct function calls to non-member functions, compiler matches them to respective function definitions at compile time.  
- Sometimes a function call can’t be resolved until runtime, this is called late binding (or dynamic dispatch in virtual function resolution).  
  - For example, when using a function pointer (type of pointer pointing to a function).  
  - Not known at compile time what function is being called, at run time, indirect function call made to whatever exists at that address.  
- Late binding is less efficient (involves an extra level of indirection).   
  - Early binding: CPU directly jumps to the function’s address.  
  - Used to implement virtual functions.

25.6:

- Virtual tables.  
- Non-virtual function, use the left side variable to determine at compile time what it should resolve to.  
- Only at runtime can the dynamic type of a variable be known.  
- Virtual table: lookup table of functions used to resolve function calls in a dynamic/late binding manner, known as dynamic dispatch.  
  - Early binding/static dispatch: direct function call  
  - Late binding: indirect function call resolution (through a pointer)  
  - Dynamic dispatch: virtual function override resolution (done through a virtual table)  
- What is the structure of the virtual table?  
  - Every class with virtual functions or derived from a class with virtual functions: has a corresponding virtual table.  
  - Static array: compiler sets up at compile tim, one entry for each function that can be called by objects of the class, each entry is just a function pointer that points to the most derived function accessible.  
- \*\_\_vptr → set automatically when class is created, points to the virtual table.  
  - Real pointer member, makes the class object allocated by a size of one pointer.  
  - It is also inherited by derived classes.  
- When an object of a class is created \*\_\_vptr points to the virtual table for that class.  
- How is each table filled out?  
  - Each table has two entries, and each entry is the most-derived function an object of that class type can call.  
- Method entries in the virtual table can point back to functions in the parent.  
- When an object is created, its virtual pointer points to the appropriate table.

25.7:

- Pure virtual function (abstract function) → no body at all, placeholder that is meant to be redefined by derived classes.  
  - Do \= 0\.   
- If we do this, any class becomes an abstract base class (cannot be instantiated).  
- Use a pure virtual function to force redefinition.  
  - Base class cannot be instantiated, and derived classes MUST define functions.  
- Pure virtual functions can be defined outside the function.  
  - Child classes can call this implementation as a base.  
  - MUST STILL BE DEFINED.  
- Interface class: no member variables, all of the functions are pure virtual.  
  - Starts with an I, define the functionality derived classes must implemented, but leave details of class implementation to the actual class.   
  - Enables freedom in parameters.  
  - Include a virtual destructor.  
- Abstract classes still have virtual tables.  
  - A constructor or destructor can call a virtual function, and needs to resolve to the right one.  
  - Entry for a class with a pure virtual function contains null pointer., 

25.8:

- Diamond structure; causes a base class to be made multiple times.  
- To prevent this, add the “virtual” keyword in the inheritance list of the derived class.  
  - Creates a virtual base class, only one base object.  
- Only constructed once.  
  - This is the one time where a non-immediate parent constructor gets directly called.  
- Virtual base classes are always created before non-virtual base classes.  
- If a class inherits one or more classes with virtual parents, the most derived class is responsible for constructing the virtual base class.  
- All classes inheriting a virtual base class have a virtual table.

25.9:

- What happens when instead of a reference or pointer to derived, we just assign a derived object to a base object.  
  - The derived portion of the object is “sliced off”; called object slicing.  
- When you accidentally pass by base value instead of reference.  
- For vector, cannot push anonymous object, and use std::reference\_wrapper to get polymorphic abilities.  
- \= operator isn’t virtual by default.   
  - Only the base portion would be copied in if a derived is set equal to a base.

25.10:

- We need access to information only in a derived class, but we only have a pointer to a base class.  
- Use a dynamic cast.   
- Can convert a derived pointer into a base pointer through upcasting.  
  - But how to convert a base pointer to derived pointer?  
  - Dynamic\_cast: enables a downcast  
- When dynamic cast fails, we get a null pointer in the conversion.  
- When does dynamic cast downcasting not work?  
  - Protected or private inheritance.  
  - Classes that’ don’t declare or inherit any virtual functions.  
  - Certain cases with base classes.  
- You can also downcast with static\_cast, but there’s no runtime checking to make sure what you’re doing make sense.  
- Can also dynamic cast with references, but if there’s some failure, an exception is thrown.  
- Dynamic cast vs static cast: static cast unless downcasting, where dynamic cast is usually a better choice.  
- Downcasting can be avoided through virtual functions.  
  - Downcasting is better however when:  
  - Such as when you can’t modify the base class to add a virtual function.  
  - Need to access something class specific.  
  - Adding virtual functions to your base class doesn’t make sense.   
- Dynamic cast uses RTTI, if you turn RTTI off, you could break something.

25.11:

- We cannot make the \<\< operator virtual.  
  - This is because it is not a member function.  
  - The parameters differ between base and derived classes.  
- Easy solution: just have the overloaded \<\< parameter call a function that can be virtualized.  
- Better: just have a overloaded \<\< function that calls a virtual function with the stream.