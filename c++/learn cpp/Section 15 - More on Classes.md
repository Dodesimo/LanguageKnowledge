15.1:

- Hidden this pointer:  
- Every member function has a this const pointer, holding address of the current implicit object.   
- Compiler rewrites expression simple.setId(2) to Simple::setID(\&simple, 2), changing simple from an object prefix to a function argument.   
- Every single member function has a single this pointer pointing to the implicit object.   
- This is just a function parameter, doesn’t make instances of class larger memory-wise.   
- Use this in a java fashion (seperate member variable from parameter)  
- Function chaining: multiple member functions called on the same object.   
  - Member function returns the implicit object as a return value.   
- This and const objects:  
  - For non-const member functions:  
    - This: const pointer to non-const value.   
  - For const member fucntions:  
    - this : const pointer to a const value.   
- Why is this a pointer not a reference?  
  - This was added to C++ when references didn’t exist. 

15.2:

- Classes and header files.  
- We can declare portions of the class outside of the class definition.   
- Putting class definitions in a header.  
  - This is possible.  
  - Entire definition of class needed.   
- Defining a class in a header doesn’t violate one-definition rule if the header is included more than once.   
- Member functions defined inside the class definition are implicitly inline.   
- Outside the class definition: not implicitly in line.   
  - Member functions defined outside the class definition can be left in the header file are made inline.   
- Put default arguments for member functions inside class definition. 

15.3:

- Nested types: can include an enum inside the class.   
- Define them at the top of the class type.   
  - Class types: act a scope region for names declared within.   
- Class types can have nested typedefs of type alises.  
- Nested classes are rare, but very possible.   
- But, the nested class doesn’t have access to this pointer of the outer class.   
- Nested type can be forward declared within the class that ecnloses it.   
  - And then can be defined outside the enclosing class.  
- Can’t forward declare nested type prior to the outer class definition.   
  - Nested type can’t be forward declared prior to the definition of the enclosing class. 

15.4:

- Destructors: clean up problem, resources need to be closed (like files, databases, network connections).   
- Opposite of constructors:  
  - Constructor: special member functions, initialize member values.   
- Destructor: function when an object of a non-aggregate class type is destroyed.  
- Rules:  
  - Destructor preceded by tilde.  
  - No arguments.  
  - No return type.   
- Destructors can safely call other member functions because the object isn’t destroyed until the destructor executes.   
- Implicit destructor created if the non-aggregate class type object has no user-declared destructor.   
  - Unhandd exceptions do notcall destructors

15.5:

- Type template parameters in member functions.   
- Defining a member function inside the class definition, template parameter belong to the class applies.  
- Outside the class definition, resupply template parameter declaration.   
- Deduction guides not needed for CTAD for non-aggregate classes, the constructor provides enough information.  
- Defining member function outside of the classroom resupply template parameter.   
  - Need to qualify with a fully qualified name of the class template.  
- Injected class name: just get rid of the type.  
  - So Pair essentially serves as a short hand for the Pair\<T\>.   
- Member function templates defined outside the class definition should be righ below.  
- Functions that are implicitly instantiated from templates: implicitly inline, so can be defined in header files. 

15.6:

- Static member variables  
- When a class object is instantiated, each object gets its own copy of all normal member variables.  
- Static member variables are shared across all objects of the class.   
- They are not associated with class objects.   
  - Static members are global variables inside the scope region of the class.  
  - Can be accessed through class name and scope resolution operator.  
- They must be defined and initialized out of the class in global scope since they are essentially global variables.   
- Non-template classes,  
  - Static member definition is placed in the associated code file.      
  - Can define the member inline and place below class definition in the header.   
  - Don’t put the static member definition in a header normally because it would result in multiple definitions and a linker error.  
- Template class:  
  - Put the member definition underneath template class definition (because these definitions are inline).   
- Static const integral types can be defined and initialized directly.  
- Inline variables: can have multiple declarations without violating the one-definition rule.  
- You can make static members inline or constexpr so they can be initilized inside the class definition.   
- Static members can use auto to deduce type for initializer or CTAD to deduce template type arguments from the initializer.   
- Static member: uses auto to deduct type from initailizer.  
- Non static members can’t use auto. 

15.7:

- Static member functions.  
- If a static meber variable is private, can’t directly access it outside the class.   
- Static member function can be used to be a getter.   
- Static member functions don’t have this pointer (since they aren’t tied to a particular object).  
- Static members can directly access other static members but not static members.   
- Can be defined outside of the class declaration (works just like every other member function).   
- All member functions defined outside the class definition are not implicitly inline, but can be made inline through inline keyword.   
  - Static member function defined in a header file: make inline.  
- Pure static classes: cannot be cloned.   
- Pure static classes: we are declaring functions and global variables in a globally accessible namespace.   
- Pure static classes vs namespaces: classes have access control, namespaces don’t.  
- C++ doesn’t support static constructors. 

15.8:

- Friend functions: function granted full access to private and protected members of another class as though it was a member of the class.  
  - Class can selectively give other classes or functions full access without impacting anything.   
- Friends cna be declared inside a class as well.   
  Friend essentially treats a function as a non-member function.  
- If you have a class forward declaration, its just the class Name;  
- Friendship doesn’t violate data hiding, because the class that has the data grants the permissions.   
- Friend function should prefer to use class interface over direct access.  
- Prefer non-friend functions over friend functions.   
  - Use accessors.

15.9:

- Friend class: can access the private and protected members of another class.   
- Friend classes don’t have access to implicit this pointer.   
- Friendship is not reciprocal or transitive.   
- A friend class declaration is a forward declaration for the class being friended.   
- References need forward declarations. 

15.10:

- Ref qualifiers:  
  - If a function returns an rvalue, the object is destroyed after initialization.  
  - This results in ref referencing a data members that was just destroyed.   
  - Essentially we want behavior where if a function is called on an lvalue, there’s no copy. However, when we call an on rvalue, we want to be safe without a dangling reference.  
  - Solve this problem through ref-qualifier.  
- Non-ref qualified overloads cannot coexist with ref-qualified overloads, so you need to specify all overloads.  
- A const lvalue qualified function accepts either lvalue or rvalue.  
- You can explicitly delete particular overloads.  
- Downsides with this:  
  - Adds clutter to the class.  
  - Results in us paying for the cost of a copy even in cases where reference was safe.