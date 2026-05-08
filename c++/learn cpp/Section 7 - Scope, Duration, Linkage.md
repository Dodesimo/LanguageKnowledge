7.1:

- Block: group of statements treated like a single statement.   
  - No semicolon at the end.  
- Can be nested  
- Have limited nesting \< 3\. 

7.2:

- Naming collision: two identical identifiers introduced in the same scope, compiler can’t figure out which one to use.   
- To avoid: define a custom namespace.  
  - Can either be upper case or lower case.   
- Accessing a namespace: use the scope resolution operator.   
- :: → can use this to do scope resolution to what’s in global  
- No id used in scope resolution, see if a corresponding one is in the same namespace, else check each other namespace to find a match.   
- Namespace also needs to be added to header files.   
- A namespace can be used across multiple files.   
- Namespaces can be nested inside other namespaces.   
  - Can be nested and aliased too. 

7.3:

- Local variables: defined within a function.   
- Scope: where you can access a variable or identifier.   
  - Compile time.  
- Local variables have block scope (only in scope from point of def to end of the block)  
- Same with parameters.  
- Variable names in a scope need to be unique.  
- Local variables have automatic storage duration  
  - They are created at the point of definition, destroyed at the end of the block.  
- Local variables also only work within their block (so outside of an inner block, they are gone).   
- In an outer block, variable can be used in inner block.   
- Local variables have no linkage: linkage determines whether a declaration of that identifier in a different scope refers to the same object or function.   
- Define variables in the most limited scope.    
- Scope vs duration vs lifetime:  
  - Scope: where the variable is accessible  
  - Duration: rules governing when a variable is created and destroyed  
    - Automatic duration: created at the point of definition and destroyed when block exited.   
  - Lifetime: actual time between creation and destruction

7.4:

- Global variables: declared outside of a function.   
  - Declared at the top of a file in the global namespace.   
  - Can be seen and used everywhere in the file.  
- Can be defined inside a user-defined namespace that is global.   
- Static duration: created when the program starts, destroyed when it ends.   
- Add g\_ to indicate global variable identifiers.   
  - Zero-initialized by default.   
- Can also be constant or constant express:  
  - Need to be explicitly initialized. 

7.5:

- Variable shadowing.   
- When a variable is inside a nested block that has the same name as a variable in an outer block, nested variable hides outer variable.   
- Same idea with global variables, but we can use :: to specify access to the global variables.   
- We shoudl avoid variable shadowing.

7.6:

- Internal linkage:   
  - Linkage: declarations of that name refere to the same object.   
  - Internal linkage: seen and used in the same translation unit, but not accessible in other translation units.   
    - Same source file.   
- Global variables with internal linkage:  
  - Make the global variable internal: static.   
  - Non constant globals have external linkage by default, but can be given internal linkage through static keyword.   
  - Const globals and constexpr globals → internal linkage by default.   
- Static: storage class specifier.   
- Function identifiers also have linkage, defaulting to external, but can be set ot internal through static.   
- Different files with same names and type of function have different functions so thus do not violate the one-definition rule. 

7.7:

- External linkage identifier: seen and used from the file in which its defined, as well as other code files through a forward declaration.   
- Functions: by default:  
  - Call a function defined in another file, place a forward declaration for the function in other files wanting to use it.   
- Make by default internal linked variables externally facing through the extern keyword.   
- Variable forward declarations: need to add extern to the variable.   
  - A forward declaration has no initialization.   
- Don’t use extern on a non-const global variable with an initializer.   
  - So only use extern for global variable forward declarations  
- Cannot froward declare a constexpr 

7.8:

- Avoid global variables.  
  - Non-const, because values can change in any function called.   
- Static initialization:  
  - Global variables constexpr initializers are initialized.  
  - Without initializers, values are zero initialized.  
- Dynamic initialization:   
  - Global variables with non-constexpr initializers are initialized.   
- We should avoid initializing objects with static duration from an another translation unit (file).   
  - Good for singleton patterns.   
- Encapsulate constant with function.

7.9:

- Function calls cause performance overhead.   
  - When a call to a function is made, CPU must store address of the current instruction and CPU registers, and jump to function.  
  - Jump back, reinstate registers, etc.  
  - There’s overhead.  
- Inline expansion: optimization where function call is replaced by the code in the called function’s definition.   
- Has a cost: can cause the executable to grow larger.  
- Compilers make decision about whether expansion should happen.   
- What function can’t be expanded?  
  - Definition is in another unit.   
  - Compiler does’ know what to replace.   
- Inline keyword: hint that expansion makes sense.   
- Not really used because its a form of premature optimiaztion and could result in misuse.   
  - Wrong level of granularity, we use inline keyword on function definition but actually per function call.   
- Norm: don’t implement function in header files because it violates one-definition rule.   
- Inline function: can be used to define function in multiple translation units/file.   
  - Must be fully defined in header file.   
  - Function can exist after if its inline.   
  - Must be the same across multiple.   
- Don’t mark functions or variables as inline unless defined in a header file.   
- Inline variables: allowed to be defined in multiple files.   
- Implicitly: static onstexpr data members. 

7.10:

- Having global constants as internal variables:  
  - Prior C++ 17, create a header file with these constants, define a namespace, add constants, include the header.   
  - Use scope resolution operator with namespace on the left, and variable name on the left to access constants.   
- Simple because const globals have internal linkage.  
- Simple since constants gets included into different code files, each of these is copied into the code file (so could be copied into multiple files, header guards can’t stop this from happening)  
  - Changing a single constant would require a recompilation of every file.  
  - Constants are large, causes a lot of memory.   
- Other solution:   
  - Have these as external variables in a C++, and then put forward declarations in a header.   
- Constexpr functions are implicitly inline, but constexpr variables are not implicitly inline.   
- If global constants, define in them in an inline constexpr global variables in a headerfile. 

7.11:

- Static has different meanings in different contexts:  
  - Global variables have static duration (created when the program starts and destroyed when the program ends)  
  - Gives a global identifier internal-linkage.   
- Static on a local variable: changes duration from automatic to static.   
  - Variable is created at the start of the program, and destroyed at the end of the program (like a global variable(.  
  - Static variable retains value despite going out of scope.  
- Static local variables that are zero-initialized or have a constexpr can be initialized at program start.  
- Static local variables that have no initializer or nonconstexpr initializer is 0 initialized at program start.   
- Good to use s\_ to prefix static duration local variables.  
- Static local variables: used when local var needs to remember its value across function calls.  
  - Static variables offer benefits of global variables while limiting visibility to block scope.  
  - So you can use it in only in the block but it still exists till the end of the program.  
- Const static local variable: allows you to initialize and create the expensive object once.   
- Don’t want to have internal states within functions, pass that in as a parameter.

7.12:

- Scope: where can you access identifier:  
  - Block local scope, only accessible till the block you are declared in.  
  - Local variables, function parameters, enums/classes  
  - Global: accessed from point till end of file:  
    - Global variables, functions, program-defined type definitions.  
- Duration:  
  - Rules that determine when it is created and destroyed.  
  - Automatic duration:  
    - Created at the definition, destroyed when the block exists.  
    - Includes local variables and function parameters.   
  - Static duration:  
    - Created when program begins and destroyed when program ends.  
    - Global variables  
    - Static local variables  
  - Dynamic duration:  
    - When programmer requests to create and destroy.  
- Linkage:  
  - Whether an identifier in a different scope refers to the same entity.  
  - Local variables: no linkage since their scope restricted to just block.  
  - Internal linkage: declaration of the same identifier refers to the same object or function.  
    - Static global variables   
    - Static functions  
    - Const global variables \+ constexpr global variables  
    - Unnamed namespaces.   
  - External linkage:  
    - Declaration of the same identifier in the program refers to the same thing.  
    - Non-static functions.  
    - Non-const global variables  
    - Extern const global variables (EXTERN CONSTEXPR CANNOT HAPPEN)	  
    - Inline const global variables  
    - Namespaces.  
    -   
- Scope: block vs file  
- Duration: automatic vs static  
- Linkage: internal vs external.  
- Forward declarations:  
  - Function forward declaration: normal  
  - You can do a forward declaration for an extern variable.   
  - Constexpr cannot happen because constexpr requires a value to be evaluated at compile time.  
  - A constexpr can be forward declared using a const variable forward declaration.

7.13:

- Qualified name: includes an associated scope.  
  - Names qualified with a namespace using the scope resolution operator ::.  
- Unqualified name: doesn’t have a scoping qualifier.   
- Using declaration: allows unqualified name to be used as an alias for a qualified name.   
  - Using std::cout.   
- Using directive: allows all identiifers in a given namespace to be referenced without qualification.  
  - Because it allows unquqliaifed access to all the names, increases the chance of collisions.  
  - Don’t cause preference for namespace over  custom.   
- C++: needs to determine what function definition matches the function call and should be used.   
- Using-statements are dependent on whether they are in a block or in a namespace.   
  - Names are applicable to the rest of the file.  
- Don’t use using statements in header files.  
  - Only use them in source files after all the \#includes.  
- Prefer explicit namespace qualifiers over using statements.  
- Avoid using-directives altogether.   
- Using declarations are for single commands, using directives are for entire namespaces. 

7.14:

- Unnamed namespacez: part of the parent namespace.   
- Content inside an unnamed namespace is treated as if there’s internal linkage (can’t be seen outside the file).   
  - Avoid unname namespaces in header files.   
- Inline namespace: used to version content.  
  - Don’t impact linkage  
  - Default to inline. 

7.15:

- Local variables: defined in a function (including parameters)  
  - Block scope: in scope from definition to end of block.  
  - Automatic storage duration: created at the point of definition and destroyed at the end of the block.  
- Global variables:  
  - File scope: in scope from point of declaration to the end of th efile.  
  - Static duration: created at program start and then destroyed when it ends.   
- Linkage:   
  - Internal linkage: identifier can only be used and see within a single file.  
  - External linkage: seen and used in defined file and other code files.   
- Can give local variables static duration through the static keyword.  
- Inline: can have multiple definitions. 
