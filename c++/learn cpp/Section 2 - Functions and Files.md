## 2.1

- Functions: collection of statements executing sequentially that are reusable.
- Function call: tells the CPU to interrupt the current function and execute another, returning back to the current function after execution.
- Function initiating the call is a caller, function being called is the callee.
- Most basic syntax:
- returnType functionName → function header
- Nested functions are illegal. 

## 2.2

- Function return values.
- Two things needed: type of value that will be returned, and a return statement. 
- A copy of the value is returned back to the caller. 
- Main cannot be called explicitly. 
- Status code: indicates whether or not the program ran successfully. 
- A value returning function must return a value of that type otherwise there will be undefined behavior. 
- Function main implicitly returns 0 if no return statement is provided. 
- Functions can only return a single value. 

## 2.4

- Function parameter: variable used in the header of a function.
- Argument; value passed from caller to the function. 
- Pass by value: arguments values are copied and passed in. 
- Unnamed parameter: parameter without a name when not necessary, C++ guide recommends using a comment. 

## 2.5

- Local variables: 
- Variables that are within the body of a function. 
- Function parameters as well. 
- Lifetime:
- Function parameters created when function is entered.
- Variables: when defined.
- Destroyed  at the end of the function. 
- Scope; where the identifier can be seen and used within the source code.
- Local variables: local/block scope, usable from the point of definition to the end of the innermost pair of curly braces containing the identifier. 
- Out of scope; anywhere a variable cannot be accessed within the code.
- Going out of scope: applied to objects rather than identifiers, end of the scope in which the object was instantiated. 
- Variables can be the same across functions in terms of names, because neither function knows that the other function has variables with the same names. 
- Use a function parameter when the caller passes in the initialization value; otherwise use a local variable. 
- Temporary object: returning from a function by value returns a temp object holding a copy of the return value. 
- No scope, destroyed at the end of the full expression. 
- Various tricks the compiler uses to avoid generation, using a return value to initialize a variable directly 

## 2.6

- Common sense as to why we would ever use a function

## 2.7

- Cant define a function after it’s been called because the compiler compiles contents sequentially. 
- Can obviously add it to the top.
- Forward declaration: tells the compiler about an identifier before defining it. 
- Also called a prototype, consists of the function’s return type, name, and parameter type, terminated with a semicolon. 
- Why forward declarations?
- Reordering may not be possible if the caller and the callee are in different files
- Enables clustering of functions that are similar.
- If forward declaration made but function not defined, if the function is never called, program will compile and run fine. 
- If its made and the function is called, but the program never defines the function, linker will complain it can’t resolve the function call. 
- Declaration: tell compiler about the existence of an identifier and associated type (just the name of the function)
- Definition: declaration that implements or instantiates the identifier (implementation of a function or instantiation of a variable)
- All definitions are declarations. Not all declarations are definitions.
- One definition rule: 
- Within a file, each function variable type or template can only have a single definition in a given scope (local variables defined inside different functions or definitions in different scopes) don’t violate this rule. 
- Causes a redefinition error. 
- WIthin a program, each function or variable can only have one definition. 
- Causes linker to issue a redefinition error. 
- Linker: connects the forward declaration to any functions with the same matches. 

## 2.8

- Compiler compiles each file individually, doesn’t know the contents of other code files. 
- Because it allows source files to be compiled in any order, when we change a file only that file gets recompiled, reduces the possibility of naming conflicts. 
- Solution: a forward declaration. 
- Each file needs to independently consider its dependencies. 

## 2.9

- Naming collision: if two identical identifiers are produced into the same program such that compiler can’t tell them apart, compiler or linker producers an error. 
- Two or more identical functions introduced into separate files belonging to the same program. Results in a linker error. 
- Two or more identical functions introduced in the same file, results in a compiler error. 
- Scope region: area of source code where all declared identifiers are considered distinct. 
- Two identifiers with the same name can be declared in separate scope regions without naming conflicts.
- Namespace:
- Allows declaration or definitions inside for the purpose of disambigulation. 
- Names are isolated from names in other scopes. 
- Global namespace: any name that is not defined inside a class, function, or namespace is part of an implicitly defined namespace (global scope).
- Identifiers inside the global scope are in scope from the point of declaration to the end of the file.
- Variables should not be defined in the global namespace.
- Can’t have executable statements in namespace. 
- C++ standard library was originally available without std:: prefix, but this meant that anything you wrote could also conflict with it. 
- :: → scope resolution operator, identifier on the left tells us what name space (qualified name). 
- Can also use a “using namespace” command.
- Problem because we can redefine functions that are in the namespace, resulting in conflicts.
- Defeats the whole point of a namespace.
- Curly braces: used to delineate a scope region nested within another scope region. 

## 2.10

- Prior to compilation, each file goes through a preprocess phase. 
- Changes are made to the text of the file through temporary files.
- Responsible for processing directives.
- #include: preprocessor replaces include with the contents of the included file. 
- #define: used to create a macro that defines how input text should be converted into replacement output text. 
- Function-like macros: serve a similar purpose as a function, generally unsafe.
- Object-like macros: replacement or substitution text. 
- Are all uppercase, separated by underscores. 
- Avoid using macros with substitution text unless no other alternatives exist.  
- #ifdef: allows the preprocessor to check whether an identifier has been previously defined, and if so code between ifdef and endif is compile.
- If 0: excludes block of code from being compiled. 
- If 0, end if. 
- Convenient way to comment out code that contains multi-line comments because they cannot be nested. 
- Macro substitution does not happen when a macro is used within another preprocessor command. 
- Include can copy over directives from an included file into the current one. 
- Directives defined in a file have no impact on other files unless they are included into another. 
- Directives are essentially unique to each file UNLESS #include. 

## 2.11

- Header files: allow for propagation of a bunch of forward declarations.
- Put declarations in one place and then import wherever. 
- #include: the preprocessor copies all of the content of the header file into the #include. 
- Include a header file with quotes not angle brackets. 
- Don’t want to put function or variable definitions in header files, violates one-definition rule. 
- Avoid including .cpp files, source files should have their corresponding paired header. 
- Latter can cause compilation errors to be caught earlier. 
- <>: tells us that the header file is something we didn’t write ourself.
- “”: tells us that the file is something we wrote. 
- Header files from other directories: set a include path or search directory in IDE. 
- Transitive includes: if you have a #includes header, other headers are also #included by that header is also added.
- Explicily mention what files you need to include through #include.
- What order should you add header files?
- Paired header file.
- Other headers from same project
- 3rd party library headers
- Standard library headers. 

## 2.12

- Header guards:
- Avoid duplicating definitions. 
- Essentially use a macro to define a variable and then do an initialization. 
- Only will get initialized if a variable is not defined. 
- Ifndef guard
- Define guard 
- Init…
- #endif
- Prevent duplicate inclusions because the first time a guard is encountered, guard macro not defined so guarded content is included. 
- This doesn’t preven a header from being added to multiple different code files tho. 
- This is because definition macros don’t persist across files unless something is explicitly included. 
- Alternative: use pragma once. 