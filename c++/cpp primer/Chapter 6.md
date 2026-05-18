- Functions:
	- Invoked with call operator, pair of parentheses.
	- Inside parentheses, comma-separated list of arguments used to init. function parameters.
- What does a function call do?
	- Initialize function's parameters from arguments, transfer control to that function.
	- First thing that happens: variables created, initialized by arguments. 
- Execution of function: ends when return statement encountered.
	- Returns value in the return + transfers control out of called function to calling function.
- Arguments: initializers for function parameters.
	- Can be evaluated in any order.
	- Type of argument matches corresponding parameter.
	- Must pass exactly same # of arguments as function has params.
- Function parameter list can be empty, can also use `void` to indicate no params.
- Two parameters of same type: type must be repeated.
	- Local variables at the outermost scope of the function may not use the same name.
- Don't need to specify parameter name, just specify the type (the # of arguments that a call must supply doesn't change).
- Return type can't be array or function type.
	- Can return a pointer to array or a function.
- Scope: part of program's text where name is visible.
- Lifetime: time during program's execution that object exists.
- Parameters/variables inside a function body:
	- Referred to as local variables, hide declarations of the same name made in an outer scope.
- Objects defined outside function: exist throughout program's execution.
	- Objects created when program starts, not destroyed until program ends.
- Ordinary local variables: created at init point in function, destroyed when at the end of the block.
	- Objects only existing in a block: automatic objects.
	- Values of these objects are undefined after execution exits a block.
- Parameters: automatic, destroyed when function terminates.
- Automatic objects for function parameters are initialized by arguments passed in.
	- Uninitialized local variables have undefined types.
- `local static`: lifetime continues across calls, not destroyed when a function ends, but destroyed when program terminates.
	- It gets initialized the first time. 
	- value initialized it there's no explicit initializer.
- Function: defined once, but declared many types.
- Function declaration: like a definition, but there's no function body.
	- Semicolon replaces function body.
- Three elements: return type, function name, and parameter types, describe function's interface.
	- Prototype.
	- No need to name parameters, but helpful to explain what function does.
- Put function declarations in a header file.
	- Source file that defines function should include header that contains function's declaration.
- Separate compilation: split programs into several files, each of which can be compiled independently.
	- Change only one source file, recompile the one that changed.
	- Separately compile each file: yields `.o`, file contains object code.
	- We can then link object files together to form executable.
		- ```zsh
		  CC -c factMain.cc # generates a .o obj for factMain
		  CC -c fact.cc # generates fact.o
		  CC factMain.o fact.o # links the two files
		  ```
- Argument passing:
	- Each time function gets called: parameters created + initialized by arguments in the call.
		- Parameter initialization works the same way as variable initialization.
	- If parameter is a reference, bound to argument.
		- Else, argument is copied.
	- Parameter and argument become independent objects, so arguments are passed by value.
	- Pointer parameters: 
		- Copy: two pointers are distinct, pointer gives indirect access to object (can alter it through the pointer).
		- Pointer parameters to access object outside a function: C
		- Reference parameters: C++
	- Operations on reference: operations on object reference refers to.
		- ```c++
		  void reset(int &i) {
			  i = 0;
		  }
		  ```
		- when we call reset, we pass an object directly, don't pass in an address.
	- Use reference to avoid copies of large class types or containers.
		- Or if can't be copied: some class types (such as IO types).
		- Use references to const for reference parameters we don't want to change
	- Use references to const for reference parameters not needed to be changed.
	- To account for multiple possible outputs, pass in parameters by reference that can be modified.
	- Top level consts are ignored when init. parameters.
		- Can pass in a const or nonconst object to a param. w/ a top-level const.
	- Top level consts do not disambiguate functions that have identical parameter lists.
	- Reference binding: only on exact type of the reference, can't be a literal expression evaluating to the type, object requiring conversion, or `const` version.
	- Reference to `const`: can be initialized from literals.
	- Array parameters:
		- Cannot copy an array.
		- Use an array, converted to a pointer.
		- Can't pass an array by value (since we can't copy it).
		- Passing an array to a function, we are passing pointer to first element.
		- All equivalent declarations;
			- ```c++
			  void print(const int*)
			  void print(const int[])
			  void print(const int[10])
			  ```
		* Since arrays decay to pointers in functions, need additional information about the size of the array.
		* Approach 1: use a marker to specify end of array:
			* C-style character strings: have a null character.
			* Harder for ints where everything could be a legit value.
		* Approach 2: standard library conventions
			* We pass in pointers to the first and one past the last element in the array.
		* Approach 3:
			* Pass in the size of the array.
	* When function doesn't need write access to array elements, array parameter should be a pointer to const.
		* Otherwise, plain pointer.
	* Reference for an array: `int (&ref)[10]`
		* This declaration tells function size of array, but limits inputs to just 10 element arrays of ints.
	* Passing a multidimensional array: 
		* Pointer to first element: so pointer to array.
		* Pass in a pointer to to array.
	* Command line arguments:
		* `int argc`: number of arguments
		* `char *argv[]`: array of pointers to C-style character strings.
			* When arguments passed, first element points to the name of the program.
			* Or passes to empty string.
			* Subsequent elements pass arguments on the command line.
			* Element past the last pointer is a 0.
	* Two ways to write function taking in a varying number of arguments.
		* Same type: pass a type called initializer_list.
		* Different types: special kind of function known as a variadic template.
		* Use elipses: `...`
		* `initializer_list`:
			* default initialization: empty list of elements.
			* `initializer_list<T> lst {a, b, c...}`: as many elements as init, elements are copies of corresponding inits, elements in the list are const.
			* Copying or assigning: doesn't copy elements, copy and original share elements.
			* Have iterators and a size function.
			* Can have other parameters in addition to an initializer_list parameter.
		* elipses:
			* interface to C code, library called `varargs`.
			* Should not be used for other purposes.
			* Only used for types common to both C and C++.
			* Last element in a parameter list and can have parameters before it.
				*  no type chekcing done for arguments corresponding to ellipsis param.
* Return Types:
	* Return statement: terminates function and returns control to where function was called.
	* Return w/ no value: return type of void.
		* Not really necessary since there's an implicit return after the function's last statement.
		* Use a return to exit at an intermediate point.
		* Can also return the result of calling another function that returns a void.
		* Returning any other expression from a void function: compile time error.
	* Return w/ a value:
		* Same type as return type, or have something that can be implicitly converted.
		* Compiler attempts to ensure that functions returning a value are exited only through valid return statement.
		* Compiler may not detect error for return right before end, what happens at runtime if return isn't there is undefined.
	* How are values returned?
		* Return value is used to init. a temporary at the call site, that temporary is result of function call.
		* Never return a reference or pointer to a local object.
			* This is because local variables have local duration and get destroyed at the end of the function.
		* Call operator: has the same precedence as the dot and arrow operators, and is left associative.
	* Call to functions that return references are lvalues, other returns are rvalues.
	* When returning, can just have a list of whatever the vector is and that initializes a temporary.
	* Braced list: contains at most one value, and the value must not have a narrowing conversion.
		* If function returns a class type, class defines how initializers are used.
	* One exception to rule that function w/ return type other than void must return value.
		* Main function can terminate without return (implicitly compiler inserts return 0).
	* Non-zero return value has machine-dependent meaning: to make return values machine independent, `cstdlib` has preprocessor variables used to define success or failure.
	* Can't do recursion on the main function.
	* Because array can't be copied, function can't return an array.
		* But to return a pointer declaration like: `int (*func(int i))[10]`
			* this is a funct. w/ param int i that returns a pointer to an array of size 10 integers.
	*  Trailing return type: make the return type `auto`, and then after parameters, use `->` to depict the return type
		* `auto func(int i) -> int(*)[10]`, return a pointer to an array of 10 integers.
	* Or use `decltype`: but since this is an array type itself, add * to indicate returning a pointer.
	* Overloaded functions:
		* Functions w/ the same name but different param lists and appear in same scope are overloaded.
		* Functions w/ same general action, but apply to different parameter types.
		* example:
			* ```c++
			  void print(const char *cp)
			  void print(const int *beg, const int *end)
			  void print(const int ia[], size_t size)
			  ```
		- main can't be overloaded.
		- Compiler uses argument types to figure out what function to call.
		- Overloaded functions must differ in # of params, or types.
		- Can't just differ based on return value.
		- Parameter lists: equal disregarding name and type aliases.
	- Parameter w/ a top-level const is indistinguishable from one w/o top-level const.
		- But can overload w/ respect to whether reference is to a const or nonconst version of a given type.
		- Remember: low level consts to the LEFT.
	- use `const_cast<>` to deal with parameters that are not const and results that we don't want to be const
		- Even though technically non-const params can bind to const params.
- Function matching/overload resolution: function call is associated w/ specific function from a set of overloads.
	- compiler determines what function to call based on params in overload set
-  for any given call to an overloaded function, three outcomes:
	- compiler finds a function that is a best match, generates code to call that function
	- compiler doesn't find a match, issues an error message
	- multiple matches, so we get an ambiguous call.
-  Name in an inner scope hides use of name declared in outer scope.
	- so if function declared outside another function, and local variable mirrors function name inside function, then function is invisible.
	- Inside scope is greater priority than outside scope.
		- Compiler ignores use of name in any outer scope.
	- name lookup happens before type checking.
- Default argument: 
	- Parameters: given a default value.
	- For each parameter int eh argument list, use = and set value.
	- Parameter has default argument, everything else following it must have defualt arguments.
	- Can only commit trailing arguments.
		- So overriding particular values are only done left to right.
		- ```c++
		  string screen(szx ht = 24, sz wid = 80, char backgrnd = '')
		  window = screen(); // equivalent to all defaults
		  window = screen(66) //left most value gets updated
		  window  screen(66, 256, '#') // everything gets updated
		  ```
* subsequent declarations:
	* have a default specified only once in a given scope
	* any subsequent declaration adds a default only for a parameter that hasn't had a default specified (cannot change a previous established default)
		* compiler amalgamates all defaults. 
* default argument initializers:
	* local variables can't be used
	* default argument can be any expression that can be convertible to type
		* Can't use a local variable to change the value of default initializers.
* Function calls are slower than just writing expression out.
	* Function call: requires register saving, argument copied, lots of extra work. \
* `inline`: expand the call inside the line.
	* use the `inline` keyword before the return type (only request to compiler, could choose to not inline it).
	* recursive functions: not inlined.
	* Optimization mechanism to optimize small, straightline functions called frequently.
* `constexpr` functions:
	* used in a constant expression
	* certain restrictions:
		* must meet certain restrictions: return type and type of each param. is a literal type
		* function body must have one return statement.
	* when possible, `constexpr` functions are replaced with their resulting value, they are implicitly inline.
		* `constexpr`functions aren't required to return constant expressions (have the potential to).
		* however, if we assume that it returns a 1constexpr and use it in a situation like that but it isn't actually const, then we have an error.
* `inline` and `constexpr` may be defined multiple times in a program (compiler needs definition/not declaration to expand code), but all definitions must match exactly.
	* so they are defined in headers.
* `assert`: evaluate expression at runtime, if true do nothing, else terminate program.
	* don't have variables that share the same name as a macro.
* `NDEBUG`:
	* if defined, assert does nothing.
	* But if it isn't, we do run-time checks.
* Need to identify what functions match.
* First step:
	* Identify set of overloaded functions considered for call (candidate functions).
		* Function w/ same name as called function and for what declaration is available at point of the call.
* Second step:
	* Select from set of candidate functions functions called with arguments. 
	* Pick functions that can be called with the arguments in the given call.
		* Viable functions: same # of parameters, type of each argument must match or must be convertible.
		* When function has default arguments, call may appear to have fewer arguments than reality.
	* If there are no viable functions, compiler errors.
* Third step:
	* Find the best match: for each argument in the call and select the function that yields best match.
		* Exact match better than conversion.
	* Multiple parameters, overall best match if and only if:
		* match for each argument no worse than match required by any other viable function
		* at least one argument for which match is better than the match provided by any other viable function.
		* if each parameter is a better match than the other viable function, ambiguous. 
	* Match priority list:
		* Exact:
			* Argument and param type is identical
			* Argument is converted from array or function to pointer
			* Top level const is added or discounted.
		* Match through const conversion.
		* Match through promotion.
		* Match through arithmetic or pointer conversion.
		* Match through class-type conversion.
	* Given a function w/ `short`and`int`, short only gets called for exact matches.
		* Int is for all promotions for small integral types.
		* all arithmetic conversions are treated equivalent to each other.
	* `const`ness: binding an object to non const reference takes precedence over binding an object to const reference if the object isn't const.
		* if const, can only bind to const refernece. 
* Pointer to function: pointers to particular function, points to particular type.
	* function type: determined by return type and type of parameters.
	* function prototype: determined by return type, name, and type of parameters
* given a function `bool lengthCompare(const string &, const string &)`, a point to this is:
	* `bool (*pf)(const string &, const string&)
* when name of function as value, function is converted to a pointer.
* can use a pointer to the function to call the function (no need to dereference pointer).
* no conersion between pointers to one function type and pointers to another (but can assign nullptr or a zero-valued integer const expression to a function pointer)
* with overloaded functions, compiler uses type of the pointer to determine what overloaded function to use. 
* can pass in a function as an argument directly or in pointer format. 
	* remember that `decltype` doesn't automatically return pointers.
	* function type automatically converts to pointer type, but when returning, need to specify pointer.
	* use alias, or just use trailing return.
* decltype: returns a function type not a pointer to function type, so need to add a *. *