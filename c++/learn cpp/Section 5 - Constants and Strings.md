## 5.1

- Constants:
- Named constants: have an identifier
- Literal constants: do not have an identifier.
- Can do named constants with:
- Constant variables
- Object-like macros
- Enumerated constants
- Named constants: add const. Add const before. 
- Must be initialized when you define them.
- Can be initialized with a non-constant value.
- Dont use const for value parameters or const when returning values. 
- Don’t use preprocessor macros for named constants:
- Macros don’t follow normal C++ scoping rules, results in all subsequent occurrences of the macro to be replaced.
- Leads to strange compilation errors. 
- C++ has only two type qualifiers: const and volatile, and type qualifiers modifies how the type behaves. 
- Volatile: tells compiler that an object’s value may have changed at any time. 
- Cv-unqualiified: type with no type qualifiers
- Cv-qualified: type with one or more type qualifiers.
- Possibly cv-qualified: could be cv unqualified or cv qualified. 

## 5.2

- Literals: values inserted directly into the code.
- Type of a literal: deduced from a value
- Can add a suffix that changes it to the data type you want it to be interpreted as.
- Suffixes are not case sensitive, but can’t have two consecutive characters with different casing. 
- Floating point literals are by default doubles.
- Make them floats: use f suffix.
- This helps avoid compiler warning with possible loss of precision. 
- All c style strings have an implicit null terminator. 
- C-style string literals are constant objects made at the start of the program.

## 5.3

- Binary and hexadecimal: prefixed with 0b for binary, 0x for hexadecimal.
- Can use a ‘, as a digit separator. Cannot occur before the first digit of the value.
- Can change output through std::dec, std::oct, std::hex

## 5.4

- Optimization:
- Can be done through hand through a program called a profiler.
- See how long various parts of the program to run, and which are impacting overall performance. 
- Optimizers: know whats the most optimal operation for a particular system.
- As if rule: a compiler can modify a program however it likes in order to produce more optimized code, as long as the modifications don’t impact a program’s observable behavior. 
- Notable exception: unnecessary calls to a copy constructor. 
- Compilers can partially or fully evaluate certain expressions at compile instead of run time: compile time evaluation
- Enables expressions to not longer be need to be evaluated at run time, executables are faster and smaller. 
- Constant folding: compress 3 + 4 to 7.
- Constant propagation: compilers replace variables known to have constant values with their values.
- Dead code elimination: compiler removes code that may be executed but has no impact on the program’s behavior. 
- A variable that gets used in dead code elimination: optimized away.
- Compilers can be more efficient at optimization by using constant variables wherever possible. 
- This tells the compiler to do constant propagation. 
- Optimizations; makes programs faster but makes it harder to debug programs.  
- Two types of constants:
- Compile-time constants: value is known at compile time
- Literals
- Constant objects whose initializers are compile-time constants.
- Runtime constant: constant whose value is determined in a runtime context.
- Constant function parameters, constant objects whose initializers are non-constants or runtime constants. 

## 5.5

- Constexpr: expression must be evaluated at compile time. 
- As-if rule: no guarantees as to what the compiler will actually evaluate at compile time. 
- We can be explicit with what parts of the code we want to execute at compile time.
- Enables for boosts in performance (smaller and faster evaluation), versatility (always use code in places requiring a compile time value), predictability (have the compiler halt compilation), quality (detect programming errors and undefined behaviors).
- Constant expression: non-empty sequence of literals, constant variables, operators, and function calls that can be evaluated at compilation time.
- What can be in a constant expression:
- Literals
- Expressions with constant operands (3 + 4, 2 * sizeof(int))
- Constant integral variables with a constant initializer. (historical, modern c++ prefers const expr)
- Constexpr variables, constexpr function calls
- What cant be in a constant expression:
- Non-const variables
- Const non integral variables (like doubles)
- Function parameters, operators with non constant expressions.
- Return value of non-constexpr functions is runtime expression, even if the function returns a constant. 
- Operators without constant expression operands can only be used in runtime expressions. 
- Constant variables that are initialized with a non-constant variable cannot be used in constant expressions. 
- Compiler is only required to evaluate constant expressions at compile-time in contexts requiring constant expression.
- Compiler only needs to evaluate constant expressions at compile-time in situations requiring a constant expression.
- Forces a compile-time evaluation if the variable is denoted to be constant expression. 
- Compile time expressions need to be constant because code could break if later on the variable gets changed. 
- For something to be evaluated at compile time, we need to for sure know what that value will evaluate to.

## 5.6

- Constexpr:
- Const variable with an integral type and a constant expression initializer can be used in a constant expression. 
- All other variables that are const cannot be used in const expressions (like float, double initialization)
- Constepr variable: keyword that modifies a type and will always result in a compile-time constant.
- Needs to be initialized with a constant expression.
- Return values of functions are not constexpr (even when the return expression is constant value), this is because functions normally execute at runtime. 
- Difference between const and constexpr:
- Const: value of the object can’t be changed after initialization. Value of the initializer can be known at compile time or runtime. 
- Constexpr: object can be used in constant expression, value of the initializer must be known at compile time. Can be evaluated at runtime or compile time.
- constexpr variables are implicitly const, const variables are not implicitly constexpr (unless they are const integral variables with a constant expression initalizer). 
- Functions:
- Const function parameters are runtime constants.
- Function parameters cannot be constexpr, since initialization isn’t determined till runtime.
- Constexpr function: function that is called in a constant expression, must evaluate at compile time when the constant expression it is part of evaluates at compile time. 
- Otherwise can be evaluated at either compile-time or runtime. 

## 5.7

- C style strings: behave oddly, hard to work with, dangerous. 
- Can’t just assign a C style string a new value, and are also dangerous (can’t just copy a larger C-style string into space allolcated for a shooter C-style string).
- Instead, use std::string, std:;string_view.
- std:;string: just another object, 
- Can initalize and change values. 
- Can handle strings of different lengths. 
- To get an entire input of string, use std::getline()
- Std::ws: ignore leading white space. 
- If using std::get line for string reading, use std::cin >> std::ws to ignore leading white space.
- >> ignores leading white space.
- std:;getline() does not ignore leading white space. 
- Std::string does not include implicit null terminator character.
- Member function
- Initializing std:;string is expensive, don’t pass std::string by value.
- “Hello”s → std::string {“Hello”, 5}
- Initializing a std::string with a literal is very very slow.

## 5.8

- Std::string_view: provides read only access to an existing string without making any sort of copies. 
- Useful when passing in strings just for function parameters.
- String view can be initialized with a C-style string, a std::string, or a std::string_view. 
- Both C-style strings and std:string can be implicitly converted into std::string_view. 
- Both std:;string and std::string_view, or C-style strings can be accepted as arguments into a C-style string. 
- However, std::string_view cannot be converted into std:;string, this is to prevent expensive copies. 
- Constexpr std::string_view
- Support for it only not std:;string. 
- Std:;string: sole owner (memory revered for a string object).
- Copies initialization values given in memory, no longer reliant on initializer in any way. 
- Std:;string: owner, responsible for acquiring string data form initializer, managing access and disposing of the string data when the object is destroyed. 
- Viewer doesn’t have any of this responsibility, provides a view of the string.
- Dependent on the object being viewed (if the object is modified, view is fried).
- A std:;string_view viewing a string is called a dangling view. 
- If function returns string, string_view makes a view of the temporary return value. Howeer, temp return value is destroyed so undefined behavior;. 
- Can’t use a std:;string_view with a std::string literal since it creates a temp value. 
- Reassignment of string causes string_viewer to also fade. 
- Potential chance that data is copied over to a new address 
- Need to be careful when returning a std::string_view.
- Returning std::string_view that views a local variable causes the returned string view to be invalid. 
- Local variables are destroyed, so string_view returns garbage values. 
- C style string literals exist entire program, so return string::view of type string view. 
- Can also return a function parameter of std::string_view. 
- This is because the caller owns the varaibles that are being passed into the stringview, so they don’t get destroyed. 
- This doesn’t work if thea arguments are temporary objects (the string_view return needs to be used in the same expression). 
- String view can me modified by removing prefix and removing suffix. 
- String_view for viewing substrings.
- This means the string_view could be viewing a non null-terminated string (c-style and std::string is always null terminated)
- To make it null terminated, convert it to a string. 
- Std::string: 
- Use when u want a string to modify, store input, store a return value. 
- Std:;string_view:
- Read only, symbolic instant, viewing the return value
- Function params:
- Std::string → function modifies string
- Std::string_view: function needs a read only string. 
- Return types;
- Std::string → return value is std::string or function parameter, return value is a function that returns that by value.
- Std:;string_view: returns a C-Style string literal or local string view. 
- Returns a string_view parameter. 