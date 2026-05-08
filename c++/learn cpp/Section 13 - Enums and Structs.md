13.1:

- User-defined types: allow creation of new custom types that can be used in programs.  
- Two different categories of compound types:  
  - Enumerated types (unscoped and scoped enumerations)  
  - Class types (structs, classes, and unions)  
- Program-defined types: must be defined and given a name before they can be used (type definition)  
- Type definitions: need to be ended with a semicolon.  
- Name with them starting with a capital letter.  
- Every code file: needs to see the full type.  
  - Forward declaration is not enough.  
- Add them in header files.  
- Types are partially exempt from one-definition rule: a given type can be defined in multiple code files.  
- Syntax:  
  - Fundamental: a basic type built into the language  
  - Compound: a type defined in terms of other types  
  - User defined: class type or enumerated type (including those in the standard library)  
  - Program defined: class type or enumerated type

13.2:

- Enumeration: compound data type whose values are restricted to a set of named symbolic constants.  
  - Two types in C++: unscoped, scoped  
- Literally just enum Name {}, and inside the curly braces we have values, with an optional trailing comma.  
- Enumerators: implicitly constexpr (makes sensee because they are predefined so we can easily figure out an enumeration’s value at compile time).  
- Enumeration: program-defined type, enumerator: specific value  
- Name enumerated types with capital letter, name enumerators with a lower case letter.  
- Each enumerator value is associated with an index.  
- Scope of unscoped enumerations: put the enumerator in the same scope as the enumeration definition itself.  
  - Enumerator name can’t be used in multiple enumerations within the same scope.  
- Can access enumerator values with scope resolution operation from enumeration.

13.3:

- Enumeators: have a value thats of an integral type.  
- Each enumerator is associated with an integer value that’s based on its position in the enumerator list.  
- Can give custom values to enumerators, but its pointless because the two become indistinguishable.  
- Value initialization/zero-initialization for an enum, defaults to the 0 meaning of the enumerator.  
- Because of this, add an invalid or unknown enumerator state with value of 0\.   
- Unscoped enumerations: implicitly convert to integral values.  
  - This is a constexpr conversion.  
- Compiler seeds if object of type enum can be printed, if not, then see if the an object of the integral type can be printed.  
- What is the integral type of enumerators?  
  - Defined as the enumeration’s underlying base.  
  - Implementation specific.  
- Integer does not get converted to unscoped enumeration.  
- Only static cast to enumerators values within the range of the smallest number of bits that holds values of all the enumerators.

13.4:

- How to print: use a switch statement,.  
- Because C-style string literals exist for the entire program, you can return a std::string\_view.   
- Can static cast an integer to a pet.  
- Getting an enumeration from a string.  
  - This is hard.  
  - Just use a series of if statements, and return a enum.

13.5:

- Operator overloading: define overloads of existing operators such that they work with program-defined data types.  
- Process:  
  - We define a function with the name of the operator as the function’s name.  
  - Add a parameter for each operand (one of the operands needs to be user-defined type)  
  - Set the return type and then return it.  
- What happens when we do std::cout \<\< 5?  
  - Std::cout type, std::ostream   
  - 5: type of literal int.  
  - When this expression is evaluated, we look for an operator \<\< function handling arguments of type std::ostream and int.   
  - \<\< function returns left operand.

13.6:

- Issue with unscoped enumerations.  
- If we are comparing two enum members, and we don’t know how to compare them, both are converted to integers.  
  - Makes no sense because they are from different enumerations and should not be able to be compared.  
- Also pollute namespace: unscoped enumerations put their enumerators in the same scope as them.  
- Scoped enumerations:  
  - Two primary differences: they don’t get implicitly converted to integers, and enumerations are only placed into the scope of the enumeration.  
  - Not where the numeration is defined.  
- Requires a scope resolution operator to access.  
- Also creates an inbuilt namespace.  
- Can do a static ast from an integer to a scoped enumerator.  
- Using enum statements: imports all the enumerators from an enum into the current scope, when used with an enum class, we do’nt have to prefix with the enum class. 

13.7:

- We sometimes have a series of related pieces of data that we need to structure.  
- Struct (structure) → program-defined data type that allows us to bundle variables into a single type.  
- Variables, function, or type inside a struct: data members.  
- How to access specific member variables: use the member selection operator.

13.8:

- For a struct, data members are not initialized by default.  
- General programming: aggregate data type allows multiple data members   
- What is a C++ aggregate?  
  - A C-style array or a class type (struct, class, or union) that has no user-declared constructors, no primate or protected non-static data members, or no virtual functions.   
- A struct with only data members: aggregate.  
- Aggregate initialization: we can pass in an initializer list.  
  - Does a memberwise initialization.  
- Missing initializers:  
  - If the member has a default member initializer, use that.  
  - Else, value initialize it.   
- You can over load \<\< to print a struct.  
- Variables of a struct type can be const.  
- Designated initializers in C++20: allow you to explicitly define when values map to what members.   
  - This is a little messy, so you can just add a new member to the bottom of the definition list.  
- You can update variables through assignment lists.  
- You can do copy initialization, direct initialization, and direct list initialization with structs.

13.9:

- Default member initialization: provide a default value for each member as part of the type definition.  
- Explicit initialization values take precedence over default values (this makes sense).  
- Default values take over when there’s missing initializers in an initializers list.  
- Two possibilities:  
  - When an aggregate is defined with an initialization list:  
    - If explicit value exists, we use that.  
    - If we have an initializer missing and a default member exists, we use that.  
    - If there’s no initializer and no default member exists, we use value initialization.  
  - When an aggregate is defined with no list:  
    - If there’s a default member, we use that.  
    - If there is not default member, the member remains uninitialized.  
- Make sure to provide a default value for all members.  
- Default initialization vs value initialization.  
- Always pick value initialization because for safety purposes, we are ensuring that any members that lack default values are value initialized.

13.10:

- Pass in structs by reference.  
  - We only need one parameter to pass in the entire struct object.   
- We can also pass temporary objects in as a parameter through initializer list.  
- Temporary objects:  
  - Created and initialized at the point of definition.  
  - Destroyed at the end of the full expression.  
  - Evaluation of a temporary object is an r value expression, so we can only pass into pass by value and pass by const reference, excludes pass by non-const reference and pass by address.   
- Structs defined inside functions: returned by value to prevent a dangling reference.

13.11:

- You can nest structs.  
- All structs that are owners of data should have data members owners.  
- This prevents dangling behavior.  
- Size of a struct is at least as large as size of all variables.  
  - Compiler can add padding when member variables are not being ordered in size correctly.

13.12:

- Member selection operator can’t be directly used on a pointer to a struct.   
- Can dereference and then do ., or \-\>.   
- Type definitions can’t be overloaded:  
  - Compiler treats the second definition as an erroneous redecaration.  
  - Overloaded functions cant be differentiated by return type  
  - Lots of redundancy.  
- Class template: template definition for class instantiation.  
- We can define multiple type names for a struct.   
- To be more generic, we can just add a single type template parameter as a parameter, will evaluate only if the fields inside has whats being requested.   
  - Don’t shadow template parameter names.  
- Std::pair: a standard struct that has two members of different types.  
- Add class templates in headers.

13.14:

- Class Template Argument Deduction  
  - When instantiating an object from a template, compiler can deduce what the types are.  
  - Class template argument deduction.  
- This only happens when there isn’t a template argument list.   
- In C++ 17, CTAD can’t deduce template arguments for aggregate class templates.  
- Deduction guide: way to tell the compiler that if it sees a declaration of something with two arguments, deduce to a particular type.  
- Non aggregates don’t need deductio nguides because a construction serves the same purpose.  
- Can give type template parameters default values.   
- When defining non-static members of a class, CTAD doesn’t work.  
  - Define each template argument explicitly.   
- Can’t be used for function parameters (so you need to add a template).   
- Creating an alias for a particular class template: works as normal.   
- Alias template:  
  - Just add a template to the top.   
  - Then a using Alias Name \= Pair\<T\>.  
- Alias template deduction: deduce type of template arguments. 