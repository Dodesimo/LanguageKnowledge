12.1:

- Fundamental data types are limited in terms of scalability and scope.  
- Use compound data types:  
  - Types defined in terms of other existing data types.  
  - Functions  
  - C-style arrays  
  - Pointer types  
  - Pointer to member types  
  - Reference types  
  - Enumerated types  
  - Class types

12.2:

- Expressions can produce side effects that outlive an expression.  
  - For example, incrementation goes beyond an expression and can impact something’s value.  
- Every expression has a type and value.  
- Type of expression: the type of the value or object or function that results from an evaluated function.  
- The type of an expression must be determinable at compile time (otherwise type checking and type deduction doesn’t work).  
  - Value can be determined at either compile time (if the expression is constexpr)  
  - Or runtime (not constexpr).  
- Value category: indicates whether an expression resolves to a value, function, or object of some kind.  
  - Right now, we will focus on lvalues and rvalues.  
- Lvalue: expression evaluating to an identifiable object or function.  
  - What does identifiable mean? Entity that can be differentiated from similar entities (through address comparison)  
  - This means that the entity can be accesses via an identifier, reference, or pointer (has a lifetime that’s longer than a single expression or statement).  
  - Thus, references a stable entity.  
  - There are two subtypes of lvalue:  
    - There’s modifiable Lvalue: whose value can be changed  
    - There’s unmodifiable lvalue: whose value can’t be modified (const and constexpr)  
- Rvalue: expression that evaluates to a value and must be instantly used.  
  - Literals (other than C-style string literals) and the return values of functions and operators that return by value.  
  - Expressions that produce temporary values that aren’t identifiable objects: rvalues.  
- Operats: expect operands to be rvalues.  
- Assignment operator: expects the left operand to be a modifiable lvalue expression.  
- How can we add variables?  
  - This is because when we have an l value provided but a r value is expected, the l value undergoes an lvalue to rvalue conversion.  
  - Thus, an l value can be used anywhere an r value is expected.  
- Why are C-style strings lvalues:  
  - Because they decay to pointers.

12.3:

- Reference: alias for an existing object.  
- Lvalue reference: alias for an existing lvalue (variable).  
- Use an ampersand.  
- Type specifying a reference (int&) → reference type.  
- Lvalue reference variable: variable that serves as a reference to an lvalue (another variable).  
  - THIS IS NOT THE SAME AS A POINTER	  
- Changing a reference changes the actual as well.  
- References must be initialized.  
- When a reference is initialized with an object, its bound to that object.  
  - Reference binding, object is sometimes called the referent.  
- You can’t have a non-const lvalue reference bound to a non-modifiable lvalue.  
  - This is because you could make changes to te reference and violate const condition.  
- Reference will only bind to an object matching its referenced type.  
  - If a bind is tried, compiler implicitly converts the object to the referenced type and bind the reference to that.  
  - Since the result of a conversion is rvalue, and a nonconst lvalue reference can’t bind to rvalue, trying to bind a non-const lvalue to an object that doesn’t match its referenced type results in compilation error.   
- A reference cannot be reset to another object.  
  - If a reference is attempted to be changed, it just changes the value of the reference and the actual object.  
- Reference follows same scoping and duration that normal variables do.  
- LIfetime of a reference adn its referent are independent.  
  - Reference can be destroyed before the object, vice versa.  
- If object destroyed before a reference, reference is left dangling.

12.4:

- An L value reference can only bind to a modifiable lvalue.  
  - So you can’t have a normal reference to a const.  
- With const keyword, we tell an lvalue reference to treat the object its referencing as a const.  
- Lvalue references to const can also bind to modifiable lvalues.  
  - You can’t modify the const reference, but you can modify the underlying variable.  
- Lvalue references to const can bind to values of different types (as long as those values can be implicitly converted to the reference types).  
  - When there is a type conversion, a reference becomes bound to a temporary object.  
- When a const reference is bound to a temporary object: we extend the life span of the temporary object other wise ref would become dangling.  
- Lvalue references: can only bind to modifiable lvalues  
- Lvalue references: can bind to modifiable l values, nonmodifiable l values, and r values.  
- Cosntexpr: allows reference to be used in a constant expression, but they have a particular limitation (they can only be bound to objects with static duration)  
  - Can only bind to a static object because we would know the memory address at compile time.   
- Constexpr references are not that useful.

12.5:

- What is the point of references?  
- Pass by value: an argument is copied into function’s parameter. So, we copy an object into a parameter, and then at the end of the function, the parameter is destroyed.  
- However, some types are really expensive to copy.   
- Instead of passing by value, we can pass by reference.  
  - Declare function parameter as a reference type rather than a normal type.  
- Pass by reference allows us to change value of argument.  
- Pass by reference to non-const is very limited because it can only bind to a modifiable lvalue.

12.6:

- If you make a reference parameter const, you can bind to any type of argument.  
  - This is because reference to const can bind to modifiable lvalues, nonmodifiable lvalues, and rvalues.  
- We are guaranteeing that we cannot change the value that’s being referenced.  
  - Allows us to pass literals to functions that used pass by reference.  
- Note: a const lvalue reference can bind to a value of a different type as long as the value is convertible to the type of the reference.  
  - This is so we can pass a value as an argument to either a value or const reference parameter the exact same way.  
- When to use pass by value vs pass by reference.  
  - Fundamental types: cheap to copy so we pass by value  
  - Class types: we want to pass by const reference.  
- What is the cost of copying an object?  
  - Size of an object: objects w/ more memory → take more time to copy.  
  - Setup costs.  
- Setting up a function call: compiler an optimize by placing a reference or copy of passed-by-value argument into CPU register.  
- Each time a value parameter is used: running program can directly access the storage location of the copied argument.  
- When a reference parameter is used: extra step.  
  - Directly access the storage location allocated to the reference.  
- For objects cheap to copy, cost of copying is similar to binding, but accessing objects is faster.  
- For objects expensive to copy, cost of the copy dominates other performance stuff.  
  - Object cheap to copy if it uses 2 or fewer “words’ of memory and has no setup costs.  
- Const reference to a string or std::string\_view?  
  - Prefer std:;string\_view, since it can handle a wider range of argument types efficiently.  
  - Cases where we should use a const reference: need to call a function that takes a c-style string or std::string since std::string\_view, not guaranteed to be null-terminated and cannot be efficiently converted to std::string.  
- Std::string\_view parameters are more efficient const std::string&.

12.7:

- Address of operator: &,.  
  - Memory addresses are printed as hexadecimal values.  
- For objects with \> one byte of memory, address of returns the memory address of the first byte used by the object.  
- Dereference (get the value at a given address as an l value)  
- Pointer: holds memory address of its value.  
  - int\* → pointer to an int value.  
- Pointers: not initialized by default → called a wild pointer (garbage address)  
  - Initialize them to a known value.  
- Type of the pointer has to match the type of the object pointed to.  
- Cannot initialize a pointer with a literal value.  
- Can mke a pointer change at what its pointing, can also change the actual value through dereferencing.  
- Pointers and lvalue references behave similarly.  
- Difference between pointers and references:  
  - References need to be initialized, pointers don’t have to.  
  - References aren’t objects, pointers are.  
  - Can’t change references.  
  - References must always be bound to an object.  
  - Pointers are more dangerous.  
- Given int x, \&x returns an int\*.  
- A pointer’s size is dependent on the architecture of the machine (32 bit machine has a 32 bit pointer).  
  - Size of pointers are always the same because memory addresses in a computer are always the same size.  
- Dangling pointer: pointer with the address of an object that is invalid.  
- Dereferencing a dangling pointer: lead to undefined behavior.  
  - Can assign an invalid pointer a new value.

12.8:

- Null value: no value  
- Pointer holding a null value: pointer is called a null pointer.  
- Use nullptr keywords for null point literal for initialization, assignment, passing a null pointer to a function.  
- Can’t dereference a null pointer.  
- Null pointer converts to false, a non-null pointer converts to true.  
- So a null pointer should either hold the address of a valid object or just be set to null pointer.  
- Avoid 0 and Null.  
- Favor references over pointers.

12.9:

- Pointers and const:  
- Can’t set an integer pointer to address of const pointer that easily.  
- Pointer to const: points to a const value.  
- Can change the actual pointer (because the pointer is not const, it just points to a const int).   
- Like a reference to const, a pointer to const can point to non-const variables too.  
  - Treats the object like a constant, regardless of whether the object at the address was defined  as a const or not.  
- Const pointer: whose address you can’t change.  
- Can declare a const pointer to a const value.   
  - Cant change address nor get value changed through pointer \=.  
  - Can only dereference it.   
- Non const pointer can be assigned another address to change what its pointing at.  
- A const pointer always points to the same address.  
- Pointer to a non-const value can change what value its pointing to.  
- Pointer to a const value: value is a const  
  - Const before \*, describes value.  
  - Const after \*, describes the actual pointer.

12.10:

- Pass by value: copy of data is passed into the function, so changes are made to the parameter.  
- Pass by reference: reference parameter is an alias for the actual parameter. Since the reference parameter is const, we are not allowed to change ref.   
- Pass by address: caller provides an object’s address through a pointer.   
  - Dereference the pointer to get access to the actual value.  
  - Copying a pointer is a lot quicker.  
- Passing by address allows the function to modify the argument’s value.  
- Prefer pointer to const function parameters over pointer to non-const function parameters unless we want to modify the object passed in.   
- Don’t make function parameters const pointers.   
  - This makes things confuinsg  
- When dealing with addresses, ensure that the pointer is not null.  
- Prefer pass by const because there’s no risk of inadvertently dereferencing a null pointer.   
  - Only lvalues can be passed by address, since rvalues don’t have addresses (thus pass by const reference is more flexible). 

12.11:

- Pass by address: allow for an optional argument.   
  - But you can just accomplish the same through function overloading.  
- To change what a pointer points at, we can’t just pass it in to the function.   
  - To do this, have a reference to a pointer.  
- Using 0 or NULL → can have unintended consequences.  
  - Nullptr always calls print(int \*)  
- Nullptr is of type std::nullptr\_t.  
- C++ just does everything pass by value (a copy is fed in). 

12.12:

- Returning by value: can cause issues if the return type is expensive.  
- Return by reference: return a reference bound to the object being returned.  
  - Return a reference to the object being retired.  
- The object returned by reference needs to exist after the function returns.  
  - Otherwise, the reference resulting will be dangling.  
- Reference lifetime extension doesn’t work across function boundaries.   
- Don’t return a non-const static local variable by reference.  
  - This is because when you increment it, and then use it to instantiate several references, all references point to the same values.  
- You can return const references to const local static variables or const global variables.   
- It’s ok to return a parameter by reference if it has been passed in as a reference.  
  - This is because in order to pass an argument to a function, the argument needs to exist in the scope of the caller.  
- Okay for an r value passed by const reference to be returned by const reference.   
  - You can return a string, then call return by reference function, then call return by reference on that.  
- Return by address also possible, allows us to return a nullptr.  
- Disadvantage of return by address, caller has to remember to do a nullptr check before dereferencing the return value.

12.13:

- In parameters: used only to receive an input from the caller.   
- Out parameter: used to return information back to the caller.  
- Disadvantages of out parameters: unnatural usage syntax, you have to create a variable, pass it in.  
- Out parameters: not obvious the arguments will be modified.  
- Using pass by address instead of pass by reference can make out-parameters more obvious.   
- Just avoid out-parameters.   
  - Prefer pass by reference.   
- You should almost always pass by const reference to avoid altering the object at hand.   
  - You should pass by non-const reference when parameter is in-out parameter.  
  - Because we are editing it anyway.  
- Use pass by non-const when a function would return an object by value but making a copy is extremely expensive. 

12.14:

- Type deduction drops const from types.  
- Type deduction also drops references.  
  - Do auto&.   
- Top level const: qualifier applying to an object:  
- Low level const: qualifier applied to the object being pointed to.  
- If we are returning a reference to const, we drop reference and then any top level const.   
- If reference dropped and then reapplied, the low level const isn’t dropped.   
- Constexpr isn’t an expression’s type so its not deduced by auto.  
- Type deduction doesn’t drop pointers.   
- Why are references dropped but not pointers?  
  - Semantics:  
    - When we evaluate a reference, evaluating the object being referenced (deducing a type, deducing the thing being referred to)  
    - Pointers: we are evaluating the pointer, not what its pointing to.  
- Auto and auto\* is th esame.  
  - auto\* the type deduced doesn’t include the pointer, so reapply it.   
- Const pointers:  
  - When doing auto const or const auto: make the pointer a const pointer.  
  - When using const with auto\*, the direction of the const tells us what to do. If its left of auto\*, we are pointing to a const thing. If its right of auto\*, the pointer itself is const.   
- Type deduction drops references and then any top-level consts.   
- Type deduction doesn’t drop pointers.  
- Auto: the deduced type is a pointer only if the initializer is a pointer.  
- Auto\*: the deduced type is always a pointer, even if the initializer is not a pointer.   
- Auto const and const auto makes the deduced pointer a pointer to const.  
- Const auto\*, makes what gets pointed to a const. auto\* const makes the pointer const. s

12.15:

- Std::optional class template type implementing an optional value.   
- Can construct a std::optional with or without a value.  
- To see if ti has a value, we can call has\_value or just add it to an if.  
  - Can deference it to get a value.  
  - .value() or .value\_or()  
- Difference between pointer and std::option.  
  - Pointer: reference semantics (references another object and assignment copies the pointer).   
  - Std::optional: value semantics, actually contains its value and assignment copies the value.   
- Can also use std::optional to denote optional parameters.   
  - Std::optional makes a copy.: this can be problematic, so only do this if you want to pass by value.    
- Just solve problem with function overloading.