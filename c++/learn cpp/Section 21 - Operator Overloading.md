21.1:

- You can also overload operators because operators are implemented as functions.  
- \+ → operator+(x, y)  
- For program defined types, need to add overloading of operators.  
- If all operands are fundamental data types, the compiler calls an existing, built-in routine.  
- If any of the operands are program-defined types, compiler uses function overload resolution to find an overloaded operator thats the best match (implicit conversions).   
- Can’t overload conditional, sizeof, scope resolution, member selector, pointer member selection.   
  - Cannot create new operator.  
- In an overloaded operator, one of the operands needs to be a user-defined type.   
  - So could be std::stirng, but use program defined types.   
- Cannot change the number of operands an operator supports, as well as default precedence.  
- So return the same thing: operators that don’t modify operands should return results by value.  
- Operators that return left most operand should return that. 

21.2:

- All arithmetic operators are binary operators.  
- Overloading arithmetic operator: define a friend function   
  - Friend function again: non-member function that has access to internals.  
- Friend functions can be defined inside the class as well.  
- When we write binary operators for operands of different types, need to write two different functions (a, b) and (b, a)  
- When chaining arithmetic operators, each one should return the same object and that becomes the left hand operand for subsequent operator. 

21.3:

- Can overload arithmetic operator normally without friend (just need a getter for internal field).  
- Friend function declaration inside the class is a prototype, you would need to provide own function prototype with a normal function. 

21.4:

- You can overload \<\<. We did this a buncho f times.  
- Need to return the output stream when chaning calls.  
  - Can’t return std::ostream by values, so return the left hand parameter (the stream) with a l value reference.  
  - Don’t need to worry about the left hand parameter existing (because the left hand parameter was passed in by calling the function).   
- Overriding input is the same, can have partial extractions, leading to failures.   
  - Make it a transactional operation (completely passes or fails).   
- Setting input stream into failure emode: std::cin.setstate(std::ios\_base::failbit). 

21.5:

- You can overload operators through a member function.  
  - Operloaded operator is a member function of the left operand.  
  - Left operand becomes an implicit \*this object.  
  - So you just do everything as if there’s only one object.  
- Friend function: operator+(cents1, 2\) (since friend defines two operands)  
- Member function: cents1.operator+(2), which becomes translated into operator+ (\&cents1, 2), since the leftmost parameter becomes \*this.  
- When to do what?  
  - Not everything can be overloaded as a friend function.  
    - \=, \[\], (), member selection are overloaded as member functions.  
  - Not everything can be overloaded as a member function.  
    - Cannot overload \<\< as a member function for example.  
    - Thiis is because the overloaded operator iss added as a member of the left operand.  
- Cannot overload the operator \+(int, centss) as a member function, as we can’t add to the int class.   
  - This is because there’s no implicit this pointer for an int class.  
- Cannot use a member overload if the left operand isn’t a class or it is an unmodifiable class.  
- When dealing with operators that don’t modify the left operand, normal/friend function is preferred.  
  - Works for all parameter types.  
- When dealing with binary operators that modify the left operand, the member function is preferred because the leftmost operand will always be a class type and it makes to have a \*this.  
- If overloading assignment, subscript, function call or member selection, use a member function.  
- Overloading a unary operator, do as a member function.  
- Overloading a binary operator that doesn’t modify left operand, use a normal function or friend function.  
- Overloading a binary operator modifying the left operand but the class definition of the left operand cannot be modified, use a normal function or friend function.  
- If overloading a binary operator that modifies the left operand and can modify the definition, use a member function.

21.6:

- Unary operators only operate on a single operand.  
- Because of this, they are implemented as member functions.  
- Same thing.

21.7:

- Overloading comparison operators.  
- Because they operate on two operands and don’t modify the left the operand, normal/friend function.  
- Only define overloaded operators that make intuitive sense.  
- Some container classes in the standard library require overloaded \< or \> so that they can keep elements in a container sorted.  
- Many of the comparison operators have a high degree of redundancy.  
- \!= → same as \!(operator \==)  
- \> —\> same as \< with the order of parameters flipped.  
- \>= is the oppososite.  
- We only need logic for \== and \<.

21.8:

- Overloading increment and decrement operators: there’s both a postfix and prefix version of each of these operators.  
- Unary, so they should be overloaded as member functions.  
- Returning \*this, allows us to chain multiple operators.  
- How to distinguish between a prefix and postfix?  
  - If the overloaded operator has an int parameter, we have a postfix overload.  
- We need to return a temporary variable when trying to overload the post fix operator.  
  - This is why postfix operations are less efficient than prefix operations.

21.9:

- We can overload the \[\] operator when trying to access members from a backing array that’s wrapped.  
- Because the result of could be used to do assignment, return an lvalue.  
- We can define a const and nonconst version of \[\] (one that allows for value changes, and an another that essentially allows for returning of values)  
- How to get rid of redundant code?  
  - Call the const code in the non-const code and use const\_cast to remove the const.  
- Why do we want to overload \[\]?  
  - We can do validity checks about array access.  
- When we call operator\[\] on a pointer to an object, C++ assumes we are trying to index an array of objects of that type.  
- We can overload with any parameter (does’nt have to be integral)  
- Make sure to dereference a pointer and then index.

21.10:

- Parenthesis operator: allows us to vary the type and \# of parameters taken.  
- Must be a member function.  
- () used to call functions in non-object oriented C++, in the case of classes () calls a function.  
- When indexing a two dimensional array, \[\] is limited to a single parameter, and is not sufficient for a two-dimensional array index.  
- However, with (), we can overload and take two integer parameters and access two-dimensional array.  
- () is used to implement functors (function objects), classes that operate as functions.  
  - Advantage of function: they store data in member variables (since they are classes).

21.11:

- Static cast: C++ doesn’t know by default how to convert any program-defined class.  
- Can use a converting constructor, but this only works if the destination is a class type where we can modify to add a constructor.  
- Add a overloaded operator with operator type() const {}, and add the return representation of the   
  - Need to be non-static members and const.  
  - No explicit parameters (but they have a hidden \*this parameter) pointing to the implicit object.  
  - Don’t declare a return type, only name of the conversion is used as return type (prevents redundancy)  
- printInt() \-\> int parameter, notices that what' s being passed is not an int, then converts into int, then passes that into printInt().  
- Explicit keyword: makes a cast only capable of being invoked through static\_cast or direct initialization.  
  - So these overloads are not considered when doing copy-initialization.  
- Make typecasts explicit.  
- Difference between converting constructor and an overloaded typecast:  
  - Converting constructor: member function that defines how B is created from A.  
  - Overloaded typecast: member function of class type A that defines how A is converted to B.  
  - Because both require definition of a member function, can only be used for modifiable class types.   
  - A converting constructr should be preferred to an overloaded typecast (cleaner)  
  - When should an overloaded type cast be used? Providing conversion to a fundamental type (since no constructor can be defined)  
  - When the conversion is returning a reference or const reference.  
  - You can add members to the resulting conversion type.  
  - When you don’t want the type being constructed to be aware of the type being convretd from.   
- When we have both a converting constructor and an overloaded typecast, both are considered during resolution.  
  - What gets picked?  
  - Depends on whether the overloaded typecast is const, the object converted is const, or what type of cast/initialization is used.  
- Thus avoid defining both type cast and converting constructor.  
- How to convert between Type A to type B?  
- If b is a class type that you can modify, use a converting constructor.   
- If a is a class type that you can modify, use an overloaded typecast to convert A to B.   
- Otherwise, use a non-member function.

21.12:

- Copy assignment operator: copy values from one object to another existing object.   
- Copy constructor vs copy assignment?  
  - Copy constructor initializes new objects.  
  - Copy assignment: we replace the contents of existing objects.  
- So, if a new object has to be created before the copying can occur, the copy constructor is used.  
  - Else, we use assignment operator.  
- Copy assignment operator must be overloaded as a member function.  
- C++: allows self.assignment, which is dangerous when we do dynamical assignment of memory.   
- We can do a simple self assignment check though (just see whether this \== \&parameter).  
- Self assignment is skipped for copy constructors (this is because the copy constructor is newly created anyway, and this is some stupid new initialization with same object thing).   
  - someClass c {c};  
- Some times self assignment is just naturally handled.   
- Copy and swap idiom: read later.  
- Compiler provides an implicit public copy assignment operator for class if a user one isn’t provided.  
  - Does memberwise assignment.   
- If class has const members, compiler defines the implicit \= as deleted, this is because const members can’t be assigned, so the compiler things your class shouldn not be assignable.  
  - If some fields are not const, overload the \= operator. 

21.13:

- Default copy constructor and default assignment operators:  
  - Memberwise copy (shallow copy)  
  - C++ copies each member of the class individually.  
  - Using the assignment operator for \= or direct initialization for the copy constructor.  
- Handling dynamically allocated memory, cant just shallow copy, shallow copies just copy the address of the pointer, doesn’t allocate new memory of copy contents.  
- Default copy will provide both with same memory address for the string.  
- Deep copy: allocates memory for teh copy and then copies in the actual value such that the copy lives in memory that is distinct from the source.  
  - So if we are copying a string, we would have to deallocate current string, then copy new string over.  
- Rule of three: if a class requires user-defined dstructor, copy constructor, or copy assignment operator, probably needs all three.  
  - This is because of dynamic memory allocation.

21.14:

- Template functions may not compile if the code tries to do some operation the type doesn’t support (add 1 to string).  
- Can defin operators such that compilation occurs.  
- So literally for any function with a template, ensure that the corresponding operators in the template have overloading for the class type.
