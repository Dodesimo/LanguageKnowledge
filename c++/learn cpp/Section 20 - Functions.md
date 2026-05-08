20.1

- Functions live at an assigned address in memory (lvalues)  
- When a function is referred to by name without parentheses, C++ converts it to a function pointer.   
  - Printing a function pointer, since function pointers are non-null pointers, evaluate to boolean true.  
- Int (\*fcnPtr) () → a pointer to a function that returns an integer.  
- C++ implicitly converts function into function pointer as needed.  
- Can dereference explicitly or implicitly.  
- When a function is called with a function pointer, its resolved at runtime.  
  - Thus, default arguments are not considered.   
-  Can be used to disambgiuate between two function calls base don default arguments.   
- Functions used as arguments to another functions are called callback functions.  
  - Example, pass in a comparison function to selection sort to determine which order things should be ordered in.  
- For a function declaration, function can be written as its prototype (without a pointer).  
- Can also specify a default function (as long as the function isn’t called with a function parameter).  
- Can also use std::function to store function pointers.  
- Std::function allows function alls through implicit dereferencing, not explicit dereferencing.

20.2:

- Segments of the program.  
  - Code segment: where the compiled program code sits.  
  - BSS (uninitialized data segment) segment: where zero-iniitalized global and static variables stored.  
  - Data segment: where initialized global and static variables are stored.  
  - Heap: where dynamically allocated variables are allocated.  
  - Call stack: where function parameters, local variables, other function related information is stored.  
-  New keyword: memory is allocated on the application heap segment.  
- Disadvantages and advantages of the heap:  
  - Allocating memory on the heap is comparatively slow.  
  - Allocated memory exists until its deallocated or the application ends.  
  - Dynamically allocated memory is accessed through a pointer (dereferencing a pointer is slowed than variable access).  
  - Since heap is large, you can store large things there.  
- Call stack:  
  - Keeps track of active functions  
  - When an application starts, main() is pushed onto the stack.  
  - When a function call is encountered, the function is pushed onto the stack.  
  - When the function ends, pop off the stack (unwind)  
- Stack itself is a fixed-size chunk of memory addresses, items are stack frames.  
  - Stack frame: keeps track of the data associated with a function call.   
  - When popping item off call stack, move the stack pointer down.  
- What is put in the stack frame (this is for the new function)?  
  - Return address.  
  - Function arguments  
  - Memory for any local variables.  
  - Saved copies of registers modified by the function that needs to be restored when the function returns.  
- Stack frame is put onto the stack, CPU jumps to the function’s start point, and instructions begin to execute.  
- Function terminates:  
  - Registers are restored, stack frame is popped (for the function that has just terminated)  
  - Return value is handled, cpu does execution at the return address.  
- Functions are pushed onto the stack when are called (with data about returning), and then popped off when returned.  
- How large is the stack?  
  - VSC Windows: 1MB  
  - G++/CLANG: 8MB  
- Advantages of the stack:  
  - Comparatively quick  
  - Memory allocated stays in scope as long as its on the stack.  
  - All memory is known at compile time. 

20.3:

- Recursion:  
  - Pretty straightforward.  
- Why does it get into infinite loop?  
  - Nothing ever gets popped from the call stack.  
- Tail call: function call that occurs at the end of a function.  
  - Easy for a compiler to optimize into an iterative function (doesn’t cause the system to run out ot stack space).  
- Don’t want to use – or \++ in expressions that are used in recursion.  
- We know what a base case is.  
- We can memo recursion (these are like leetcode basics).  
- Can always solve a recursive problem iteratively.  
  - Iterative functions are typically always more efficient than recursive counterparts.  
  - This is because pushing to the call stack requires some overhead.

20.4:

- When compile and link program, output is an executable file.  
- Program ran: execution starts at the top of main()  
  - How can you have an external program pass in data to our program.   
- What is argc?   
  - Count of the number of parameters that are passed to the program.  
  - First argument is always the name of the program itself.  
- Argv:  
  - Argument values.  
  - C-style array of pointers to characters (string).  
- Can’t use range based for loops on decayed C-style arrays.  
- OS parses command line arguments first.

20.5:

- Ellipses: allow us to pass a variable number of parameters.  
- Ellipses are rarely used, potentially dangerous, fade.  
- How are elipses using functions formatted?  
  - Return\_type function\_name (argument\_list, …), need to match the argument\_list parameters passed in.   
  - Ellipses must always be the last parameter in the function.  
- Access values through a special type called va\_list (pointer that points to the ellipsis array).  
- Then, initiailize it with va\_start(), va\_list and then last non-elipsis parameter in the function.  
- Use va\_arg to access elements, specify the list and the type of the data getting accessed, move va\_list to the nex parameter.  
- Issues with ellipsis:  
  - No type checking (type checking is suspended for ellipsis parameters)  
  - If types don’t align, we only read a certain number of bytes, so bytes get split up and garbage results are read.  
  - We also have no clue how many parameters are passed in to the ellipsis.  
  - How to fix this?  
    - We pass in a length parameter.  
    - Or we use a sentinel value, a special value used to terminate a loop when we encounter it.  
  - Or we pass a decoder string that specifies the number of numbers as well as the type of the numbers.

20.7:

- Lambda: allows us to define an anonymous function inside another function.  
  - Allows us to avoid namespace naming pollution and define function as close as possible to where its being used.   
- Format:  
  - \[ captureClause \] (parameters) → returnType {statements}  
  - Capture clause can be empty if there’s no captures needed.  
  - Parameter list → empty if no parameters are required (can also just be completely omitted)  
  - Return type is optional, if omitted, auto is assumed.  
- Function literal: defining a lambda exactly where its needed  
- Can store a lambda in a variable.  
- What type is a lambda?  
  - Lambdas don’t even have a type we can explicitly use.  
  - When we write lambda, compiler generates a unique type for the lambda not exposed.  
  - This is because lambdas aren’t functions (they are functors, containing an overloaded ()).  
- How to call a lambda in a function?  
  - Case 1: function param is a std::function, can we see the params, return type, but lambda needs to get implicitly converted.  
  - Case 2: function param is a T, we can just instiate where T matches the actual type of the lambda, but types aren’t apparent  
  - Case 3: use auto to invoke the abbreviated function same thing as 2\.  
  - Case 3: can directly pass in a function pointer (only works with lambdas that don’t have captures).  
- Generic lambda: when lambdas have one or more auto parameters (can work with a wide variety of types).  
- Lambdas are implicitly constexpr if the result satisfies requirements of a constant expression.  
  - So no captures, captures must be constexpr, and the lambda cannot be const constexpr.   
- Each version of lambda has unique static variable.  
- Return type deduction: all return statements in the lambda must return the same type.  
  - Since the return statement is used to deduce the type of what gets returned.

20.7:

- Lambdas can’t access all objects that have been defined outside the lambda.  
  - Can only access static storage duration objects (global vars, static locals)  
  - constexpr objects.  
- Capture clause, given the lambda access to variables in surrounding scope that it normally wouldn’t have access to.  
  - How does this work?  
    - Each variable that the lambda captures, clone of that variable is made inside the lambda.  
    - Clone variables are initialized from the outdoor scope variable of the same name.  
    - When a compiler encounters a lambda definition, it creates a custom object definition for the lambda, where each captured variable is a data member of the object.   
- Captures are const.  
- To allow modifications to variables that are captured, we can mark the lambda as mutable.  
- You can capture multiple variables with commas.  
- Can capture all variables mentioned in a lambda with reference by doing &, the same capturing by value with a \=.  
  - Can mix and match.  
- Defining a new variable in the lambda: in the capture, initialize without definition the value, its auto deduced.  
- If variable captured dies bfore the lambda, lambda holds a dangling reference.  
- Captured variables must outlive the lambda.  
- Copied lambdas have their own version of variables.  
- Provide lambdas with mutable captured variables, pass them by reference through std::Ref.