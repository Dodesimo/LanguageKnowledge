14.1:

- Procedural programming: create functions that implement logic, pass in data to those functions.  
  - This results in functions and data being separate.  
- In programming, objects are represented as properties, behaviors represented as functions (so procedural programming represents reality poorly).   
- OOP: create classes that contain properties and a set of well-defined behavior.  
  - So you delineate subject behavior and accessories.  
- Procedural: use enums to serve as types and use functions that take enum value in and return a string or something of that sort.   
- OOP: allows you inheritance, encapsulation, abstraction, and polymorphism.  
- Traditional meaning of object: a named piece of memory that stores values.

14.2:

- What is the issue with structs? They don’t provide a way to document and enforce invariants.   
  - Class invariant: condition must be true throughout the lifetime of an object for the object to remain valid.  
- Class invariants become more complex to maintain when members of a struct have correlated values.   
  - Relying on the user of an object to maintain class invariants results in problematic code.   
- Class: a program-defined compound type with many member variables of different types. 

14.3:

- So far we are separating properties and actions. Structs contain properties, actions define operations on the properties.   
- Class types (structs, classes, unions) can also have functions.   
  - Functions belonging to a class type are called member functions.   
  - Don’t belong to a class: called non-member functions.  
- Member functions: declare inside the class type definition, can be defined inside or outside the class type definition.   
- Member functions are declared inside class type definition.  
  - In the non-member version, the function has external linkage and could be called from other source files.   
- Member functions are implicitly inline (don’t cause violations from one-definition rule if added into multiple code files).   
- . → member selection operator.  
- Object that a member function is called on is implicitly passed to the member function.   
- Member function: any identifier not prefixed with . is associated with the implicit object.   
- Inside of a class definition: restriction doesn’t apply, you can access member variables and functions before they are declared.   
  - However, be mindful of order of declaration.  
- Member functions can also be overloaded.  
- In C; structs only had data members, not member functions.  
- C++, has functions.   
- Class type with a constructor ris no longer an aggregate.   
- Can create a class type with no data members and just member functions. 

14.4:

- Class type objects can be made const.   
- Can’t change data members of const objects.  
  - Cannot change member variables or call functions setting the value of member variables.  
- If an object is const, it can’t call a non-const member function.   
- A const member function: guarantees that it will not modify the object or call any non-const member functions.   
- Const member functions can only modify non-members (like local variables and function parameters) and call non-member functions.  
  - The CONST part only applies to members.   
- Non-const objects can call const functions.  
  - Const functions: make a function const if it does not modify the state of the object.   
- We make a parameter to a function const so that it can accept const l value arguments and r value arguments.   
- Possible to overload based on the const keyword. 

14.5:

- Each member of a class type has access level: determines who can access the member.   
- Access controls: determine whether the member can be accessed.  
- Public access level: no restrictions on access level.  
- Public members: can be accessed by the public and other members of the class type.  
  - Members of a struct: public by default.  
- Private members: members of a class type that can only be accessed by other members of the same class.   
- Cannot do aggregate initialization: because an aggregate can’t have private or protected non-static data.   
  - A class cannot qualify as an aggregate.   
- Private data members: m\_ prefix.   
  - Don’t want to shadow function parameters or member functions.   
- Structs should avoid access specifiers all together (we want them to be default public, because all structs should be aggregates).  
- Classes should generally only have private or protected data.   
  - Member functions are typically public.   
- Access levels work on a per-class basis.   
  - So a member function can directly access the private members of any other object with the same scope.   
- What is the difference between structs and classes:  
  - A class defaults to public  
  - A struct defaults to private.   
- Structs inherit from other class types and classes inherit privately.   
- Struct:  
  - Simple collection of data that doesn’t benefit from restriction  
  - Aggregate initialization: sufficient  
  - You have no invariants, setups, or cleanups.

14.6:

- Access functions: job of retrieving or changing a value of a private member variable (getters or setters)  
- Getters: accessors are functions that return the value of a private member.  
- Setters: set the value  
- Getters should be const  
  - We want const objects to have the ability to get a value.  
- Setters should be non-const.   
- Prefix with get and set.   
- Getters should either return by value or by const lvalue reference.

14.7:

- Okay to return by reference function parameters passed by reference or variables with static duration.   
- Member functions can also return by reference, following the same rules.  
- Member functions return data members by reference.  
  - Done through getter access functions.  
- Member functions can return data members through a const lvalue reference.   
  - This is because data members have the same lifetime as the object containing them.  
  - Thus, it is safe for a member function to return a data member by const lvalue reference.   
- Don’t allow unnecessary conversions, return type of a member function should match the data amember’s type.   
  - Could use auto, but for description purposes,   
- If a function returns by value an rvalue, don’t use that as a reference because what gets created gest destroyed at the end.   
  - R value gets destroyed at the end of the expression.   
- Use the return value of a member function returning by reference immediately.   
- Don’t return non-const references to private data members (this makes sense because we can edit it then).   
- A const member function can’t return non-const references to data members. 

14.8:

- Why are variables private?  
  - They allow separation of interface and impleemntation.   
- Class interface: how users will interact with objects of the class.  
  - Public members of a class form its interface.  
- Implementation: code that makes the class behave as intended.   
  - So the member variables and bodies that contain program logic and manipulate member variables.   
- Data hiding: make the implementation inaccessible.   
- Classes should use this.  
  - Structs should not because this prevents them from being treated as aggregates.  
- Encapsulation: bundling data and functions for operating on instances of that data.   
- When users get access to implementation of a class, they are responsible for maintaining all invariants.   
- We don’t have to change the public interface if we separate the interface from implementation.  
- If you have the choice between a non-member function and a member function, implement a function as a non-member function.   
  - Nonmember functions aren’t part of the interface of class, so the interface is smaller.  
  - Non member functions enforce encapsulation.  
- Declare public members first, protected members next, and private members last. 

14.9:

- When a class type is an aggregate, we can use aggregate initialization to initialize the class type directly.  
- When can we no longer use an aggregate?  
  - When the members are private.   
- So aggregate initialization does not work.  
- Constructor:  
  - Special member function called after a non-aggregate class object is made.   
- When a non-aggregate class type object is defined, compiler tries to see if there’s a matching constructor.   
  - If a constructor is found, memory for the object is allocated and the construction function is then called.   
- Constructors can do two things:  
  - Initialize member variables through member initialization list.  
  - Do setup functions.   
- Aggregates can’t have constructors.   
  - Constructors hae the same name as the class.  
  - No return type.  
- After constructor finishes exeecuting, object has been con  
- Implicit conversions work for constructors as well.  
- A constructor is not const because it alters the state of the object.  
  - Constructors are the only non-const member function that can be invoked on a constructor.  
- Constructors versus setters:  
  - Constructors initialize an entire object at the point of instantiation.  
  - Setters: assign a value to a single member of an existing object. 

14.10:

- Member initializer list: how a constructor initializes a list of values.  
- Add colon, and put member then in braces the value for it.   
- So how this works is that when instantiation happens everything in the list is initialized with the specific values, and then the body of the constructor runs.  
- Members in a member initializer list are always initialized in the order they are defined in the class.   
  - Initialization list order doesn’t matter, whatever gets initialized first is what shows up first in the class.   
- Avoid this issue by initializing members in the member initializer list in the order in which they are defined in the class.   
- You can initialize through a member initializer list, a default member initializer {value here }, or just default initialize it {}.   
  - The member initializer list takes precedence.   
- Can do copy assignment, but this doesn’t work for situations where members are required to be initialized.   
- Once member initialize list has executed, object is initialized.  
- When function body has finished executing the object is considered constructed.   
- For error handling, we can use statements inside the body fo the constructor.   
  - Throw an exception when constructor fails.   
- Why don’t we pass in std::string view  
  - Because we are passing in a temporary object 

14.11:

- Default constructor: takes no arguments.   
- Value initialization and default initialization works.   
  - Just use value initialization for everything.  
- If all the parameters in a constructor has default arguments, the constructor is a default constructor (can be called with no arguments).   
- Constructors can be overloaded like other, normal functions.   
- Class should only have on default constructor (since you can disambiguate what should be used).   
- Implicit default constructor:   
  - Has no parameters, member initializer list, and no statements.   
- When we have a user declared constructor, if we want a default construtor, what we can do is specific one through \= default.  
- Two cases where explicitly defined default constructor is diff than user-defined constructor.   
  - Value initializing a class. If the class has a user-defined default constructor, the object will be default initialized. If implicit, zero-initalized.   
- For a class that doesn’t have a user provided default constructor, value initialization first zero-initializes the class, default initialization doesn’t.   
- Prior C++, a class with a user defined default constructor made the class non-aggregate, while an explicit default constructor didn’t.   
- Default constructor: crates objects of non-aggregate class types with no user-provided initialization values. 

14.12:

- You can essentially use another constructor in another one.   
- Creating a construction in the body of a function creates a temporary object.   
- Can’t call constructor in body of another because initialization of a class object is finished once the member initializer list has finished executing.   
  - In the body of a function, a function call to the constructor creates and direct initializes a temporary object.   
- Just add constructor call in the member initializer list.  
- A constructor that delegates to another cannot do any member initialization itself.   
- Initialize one w less parameters to the one w more parameters.   
- Alternative for delegating constructors:  
  - Default initialize data members and add a default argument for parameters.  
- Just for best practice, default initialize and add as an optional parameter.   
  - Would you want redundant constructors with delegation or redundant default values. 

14.13:

- Temporary class objects.   
- You can directly pass in r-value expressions to functions if they accept them.   
- You can pass in a temporary object instead of a named variable (temp objet has no name and exists for the duration of a single expression).   
  - No name, when the function returns the temp object is destroyed.  
- Can’t create temporary object using copy initialization.   
- Can make temporary objects using direct initialization.   
- Foo(bar) is the same as foo bar.   
- Function returns by value, object returned is a temp object.   
- Temporary objects:  
  - R values.  
  - Created at the point of definition, destroyed at end of full expression.   
- Static\_cast: returns a temporary type T direct initialized with value of C.   
- T {v} → creates a temporary type T that is list-initializaed.   
- Reminder that a std::string\_view doesn’t implicitly conver to a std::string.   
  - So can static cast can be used to std:;string.   
  - Can create a temporary that’s list initialized.  
- static\_Cast when converting to a fundamental type, list-initialized when converting to a class type.   
- Static\_cast to create an object when we need to narrow, different behavior, use direct initialization. 

14.14:

- Copy constructor:  
  - Used to initialize an object with an existing object of the same type.  
- After execution, newly created object should be copy of the object passed in as the initializer.   
- There is an implicit copy constructor that does memberwise initialization (each member is initialized using a corresponding member of the class passed in as the initializer).   
- Prefer implicit copy constructor (most of the time its fine).  
- Parameter of copy constructor must be a reference.  
- When argument and parameter are the same and an object is passed by value, the argument is copied into the parameter.   
- Same thing applies to return types and return values.  
- If we don’t want copies to be made, we can do \= delete. 

14.15:

- No initializer: int a;  
- Initializer after equals sign: int b \= 5;  
- Initializer in parenthesis int c (6);  
- List initialization: int d {8} → direct list initialization, int d \= {8}, initializer in braces after equals sign, int f{} → value initialization.  
- List initialization: disallows narrowing conversions.   
- Copy initialization: considers non-explicit constructors/conversion functions  
- List iniatialization: prioritizes matching list constructors over other matching constructors.   
- Copy elision: compiler normally calls a copy constructor, the compiler is free to rewrite the code to avoid the call.   
  - Constructor has been elided.  
- Can do this optimization even if the copy constructor has side effects (even if the copy constructor has side effects).   
- In c++ 17, copy elision became mandatory. 

14.16:

- Fundamental types can be converted using built-in numeric promotion/conversion rules.  
- C++ doesn’t have specific rules to tell us how to convert values to and form a program-defined type.  
  - Look at user defined-conversion.   
- The compiler tries to see if there’s a function that allows it to convert an int value into a Foo object.   
  - This is the Foo(int) constructor.   
- When printFoo(5) is called, we create a Foo parameter F through copy initialization with Foo(5) as an argument.  
- Prior to C++7, printFoo would be converted to a temporary Foo, which would then be copy constructed into parameter f.  
  - Onwards, the copy is mandatory elided.  
- Converting constructor: a constructor that can be used to do an implicit conversion.   
  - All constructors are by default converting constructors.  
- Can only do one user-defined conversion.   
- You can convert an implicit conversion into an explicit one through direct list initialization.   
- How do you force certain implicit conversions through a converting constructor to not happen?  
  - Explicit keyword: tells constructor we should not use that keyword to convert.   
  - Explicit constructor: can’t use copy initialization or implicit conversions.  
- Explicit constructors still work for direct and direct list initialiaztion.   
- Static\_cast uses explicit constructors.   
- When returning, the value doesn’t match the return type of the function, implicit conversion.   
- If all constructors are explicit, can’t implicitly convert return type.   
- Best practice: make any constructor accepting a single argument explicit by default.   
  - If an implicit conversion between types is semantically equivalent and performance, make the constructor non-explicit.   
- Don’t make copy or move constructors are these don’t perform conversions.   
  - Processes liek value initialization are intrinsic to language and don’t work if 

14.17:

- You can make member functions constexpr.   
  - Can either be evaluated at compile-time or runtime.  
- However, when a function call is used for a constexpr, the object needs to be a constexpr as well.  
- Aggregates implicitly support constexpr.   
- Literal type: possible to create an object within a const expression.  
  - Object can’t be constexpr unless the type qualifies as a literal type.  
  - Scalar types, reference types, aggregates, classes with a constexpr constructor.  
- To allow for constexpr evaluation, make the constructor constexpr.   
  - For a class to be evaluated at compile time, make member functions and constructor constexpr.   
- When a constexpr function is being evaluated in a compile-time context, only constexpr functions can be called.   
- Constexpr member functions are not longer implicitly const, need to explicitly mark it as such.   
- Constepxr member functions can change data members (duh, since its not const).   
- You can’t call a non-const member function on a const object.   
- When evaluating in a compile-time context, can only call constexpr functions.   
- Constexpr and const are separate things.   
- Constexpr const int& getX() const {}  
  - Constexpr: we can use this in a compile-time evaluation context.  
  - Const int& → return type of the function