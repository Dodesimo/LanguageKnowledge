11.1:

- Function overloading: create multiple functions with the same name such that each identical named function has different parameter types.  
  - Each function is an overloaded function.  
- Overload resolution: when the compiler tries to match the function call to the appropriate overload based on the arguments used in the function call. 

11.2:

- Functions overloaded are differentiated based on number of parameters and type of parameters (excluding typedefs, type aliases, and const qualifiers, includes ellipses).  
  - Do not use return types.  
- Type signature of a function: only includes the function name, number of parameters, parameter type, and function-level qualifiers.   
- Name mangling: compiled name of the function is altered

11.3:

- Multiple functions could match a function call.  
- Compiler has to determine which function is the best match.   
  - This process is called overload resolution.  
- Resolving process:  
  - Each step, compiler applies a bunch of different conversions, and sees if any are now a match.  
  - Three possible outcomes at each step:  
    - No matching functions → go to the next step.  
    - Single matching function → single match, use that.  
    - More than one matching function was found → ambiguous match compile error.   
- First the compiler tries to see if there’s an exact match.  
  - See if there’s an overloaded function where type of arguments EXACTLY matches type of parameters in overloaded function.  
- Second: compiler applies a series of trivial conversions.  
  - Modify types to to find a match.   
  - Trivial conversions: specific conversions that modify types for the purposes of finding a match.  
    - Lvalue to rvalue conversions, qualification conversions (non-const to const), non reference to reference ovnersions.   
    - Trivially convert a int to const int (for ex.)  
    - Trivially convert a double to const double & (we are essentially converting a normal value to a pointer)  
  - Then, do numerical conversions.  
    - Leads to ambiguity when converting a long to a int or double.   
    - In this situation, explicitly cast to what you want the function to match to, use a literal suffix.   
  - Then, we do user conversions.

11.4:

- Delete: get rid of a function call that could happen for a particular type.   
- Delete functions participate in all stages of function resolution.  
  - If a deleted function is selected error. 

11.5:

- Default argument: default value provided for a function parameter.   
  - The caller can optionally provide an argument for any function parameter with a default argument, if it does then that custom value is used, else the default argument is used.  
- Can only be done through an equals sign.  
- Default arguments are handled by the compiler at the call site.   
  - So if print(3) is seen, and the default argument for the second parameter is 4, then the function call is rewritten as print(3, 4\)  
- Useful for cases where we need to add a new parameter.  
  - If you add a new parameter without a default argument, it breaks existing function calls.  
- As of now, there is no clear way to provide explicit values for specific parameters.  
  - Thus, in function call, any explicitly provided arguments need to be the the leftmost arguments (arguments with defaults cannot be skipped).   
  - Only parameters to the right can have default arguments.  
  - If a parameter has a default value, all susbequence parameters need to also have default arguments.  
- Once declared, a default argument can’t be redeclared in the same translation unit (so a function with a forward declaration and function definition, default can be either in the forward declaration or the function definition, but not both).   
  - Best practice: add default argument in the forward declaration, not the function definition.  
- Default values are not part of a function’s signature.  
- Can easily cause ambiguous function calls.  
- If you have two functions where one defaults to int and the other defaults to double, and you supplied just a single parameter, we don’t know what the second parameter would default to. 

11.6:

- Templates: simplify process of creating functions or classes that can work with different data types.  
- Instead of manually creating a branch of identical functions or classes, we create a single template.  
  - We can use one or more placeholder types.  
- Templates can work with types that didn’t even exist when the template was written.  
- Function templates:  
  - Function-like definition that generates one or more overloaded functions, each with a different set of actual types.  
  - We use placeholder types.  
- 3 different types of template parameters.  
  - Type template parameters: the template parameter represents a type.  
  - Non-type template parameters: the template parameter is a constexpr value  
  - Template template parameters: the parameter itself represents a template.  
- So we define a function where all types are replaced by a T.  
- We then create a template parameter definition, where we say T is referring to a type name.   
  - This is limited in scope to the template that follows.  
- Use a single capital letter to name type template parameters used in trivial or obvious ways.  
  - Else, use a more descriptive name.

11.7:

- Function templates:  
  - \<\>: defined as a template argument, specifying the actual type that is used in place of template type T.  
- Process of creating a function from a function template: function template instantiation.  
- Function instantiated due to a function call: implicit instantiation  
- Function specifically instantiated from a template: called a specialization, or a function instance.  
  - Template from which a specialization is produced, called a primary template.  
- Clone the template and replace template type with actual type.  
- Function template: instantiated only the first time a function call is made, further calls are routed to the already instantiated function.  
- Situations where the type of the arguments match the actual type wanted, don’t specify the actual type (instead use template argument deduction)  
  - Just leave the template argument blank.  
  - Or just fade it completely.  
  - With empty brackets, compiler only considers max\<int\> template function overloads.  
  - Without any sorta angled brackets, consider both max\<int\> template function overloads and max non-template function overloads.  
  - () without any angle brackets, considers non-template function that is equally vaiable.  
- When we have a normal function call syntax, we use this to call functions. Why is that?  
  - Syntax is more concise.  
  - Rare that we will have both a matching non-template and a function template.  
  - If there is a match, we prefer the non-template function.  
    - This is because that specific implementation must have a more optimization or specialized types.  
- So essentially, \<\> will not consider non-template functions. Getting rid of angle brackets just considers function.  
- Compiler: successfully compiles an instantiated function template if it makes sense syntactically, but the compiler has no way to check whether a function makes sense semantically.  
- Static local variables:  
  - When used inside a function template, each function instantiated will have a separate version of the static local variable.  
  - Not a problem if the variable is const.  
  - But this can be a probem if there are different functions getting called as they are differently instantiated from the template  
- Generic programming:  
  - Programming with templates as we aren’t worrying about the actual types.

11.8:

- If we make a function call without using angled brackets to specify a type, we look for a non-template match.   
- Next, we see if there’s a template match using template argument deduction (we try to find the type).  
  - This obviously fails because t can only represent a single type.  
  - There is no type fulfilling both 2 and 3.5 without type promotions.  
  - Type conversion is done only when resolving function overloads, not when performing template argument deduction.  
  - So we don’t do any type conversion when we are trying to figure out to infer the template argument.   
- We can solve ambiguities through static casts.  
- Or what we can do is provide an explicit type template argument.  
- We can also provide multiple template type parameters, and this allows for independent resolution.  
- Function templates: if you juse use auto for all parameters, is an implicit function template. Abbreviated function template.

11.9:

- Non-type template parameters: template parameter with a fixed type that serves as a placeholder for constexpr value that is a template argument.  
  - So it can be an integral type, enumeration type, floating point, pointer or reference to object function member function, a literal class type.  
- Name of an int non-type template parameter: conventionally N.  
- Reminder that function parameters can’t be constexpr: because we can’t evaluate all parameters for a function at compile time (what if we read input and pass that in)  
  - We can circumvent this by passing in non-type template parameters.  
- Thus, non-type template parameters are used primariuly when we need to pass contsexprt values to functions or classes so that they can be used in contexts requiring constant expressions.  
- Non-type template parameters can use auto to have the compiler figure out what the non-type template parameter is.  
  - Since this isn’t a type related thing, there are distinct function parameters. 

11.10:

- Put all template code in a header file instead of a source file.  
- This doesn’t violate the one-definition rule because identical definitions are getting copied into multiple files.  
- Functions instantiated from templates are implicitly inline, so they can be defined in multiple files.  
  - So the linker essentially merges these instances and doesn’t act up.

11.11:

- Overload resolution: figure out what function to use, favor ones that can be matched through numeric promotions instead of numeric conversions.  
- Ambiguous match: compiler finds two or more functions that can match a function call, can’t determine which one is the best.  
- When auto keyword is used as a parameter type in a normal function, compiler converts the function into a function template where each auto parameter becomes an independent template type.  
  - Abbreviated function template.  
- Non-type template parameter: allows us to pass in a constexpr value as a template argument.