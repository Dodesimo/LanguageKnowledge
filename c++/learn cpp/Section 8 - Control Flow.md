8.1:

- Execution path: specific sequence of statements the CPU executes.  
  - CPU begins execution at the top of main, executes some number of statements, terminates at the end of main.   
- Very rarely do we get a straight-line program.  
  - Path are altered with control flow statements, change the normal path of execution. (if statements).  
  - Causes branch  
- Conditional statements: execute if a condition is met.  
- Jumps: tell the CPU to start executing statements at some other location.  
- Function call: jump to some other location and back.   
- Loops: execute some sequence numerous time.  
- Halts: terminate the program  
- Exceptions: control structure designed for error handling. 

8.2:

- Basic if statements.  
- Block can be contained in braces.  
- Blocks should be used because they make it clearer.   
- If else only one true condition executes.  
- If if: execute all true conditions  
  - Return, can just use a chain of ifs. 

8.3:

- Nested if statements and dangling else problem.   
  - Dangling else: where does the else belong to?  
  - It is attached to the last unmatched if statement in the block  
- Null statement: they don’t do anything.  
  - Don’t terminate if statement with a semicolon

8.4:

- If statement is normally evaluated at run time.   
- However, when the conditional is constant, it is wasteful to evaluate at runtime.   
  - Solution is constexpr if statement.   
  - The condition must be a constant expression.   
  - If it evaluates to true, replace if else block with the contents of if.  
  - Most compilers optimize for this automatically

8.5:

- Chaining if statements is inefficient (multiple evaluations and equality checks)  
- Switch statement: easier.   
  - Evaluate an expression, then evaluate the case that matches the case label.   
  - If no match exists, and a default exists, execute the ones in the default label.  
  - If no matching value is found, skip the switch.   
- Switch condition must evaluate to an integral type (number, boolean)  
  - This is because switches are jump table was how compilers implemented switches.  
- Case label: constant expression needs to match the type of the condition, or must be convertible to that type.   
  - If the value of the conditional expression equals the case label, we go there.  
- Case labels need to be unique.  
- Default label:  
  - If the conditional expression does not match any case label and a default label exists, we go there.   
- If there’s no matching case label or a default case, we go to after the end of the switch block.   
- Break: leave the switch cand continue with statements after the end of the switch block.  
  - Make sure each set of statements in a label either breaks or returns.   
- Don’t indent labels: this is because all statements are part of the scope of the switch bloc.   
- Switch vs if-else:  
  - Switch tests for exact equality for a single expression across a small number of values.   
  - If else is better suited for testing expression for comparison other than equality, testing multiple conditions, as well as other types. 

8.6:

- Since everything under the case is executed, fallthrough is when everything under the case is executed.   
  - This happens if there’s no continue or breka.   
- Sometimes we want to allow faillthrough, use an attribute (gives the compiler additional data about the code).   
- Can stack labels to validate truth.  
  - For a series of cases that are “stacked” then at the end we have return True.   
  - Default: return false.   
- Labels do not define a new scope.  
- All statements under a case are scoped as part of a switch.   
- Can declare or define variables inside the switch both before and after case labels.  
  - Not initialize though.   
  - Make an explicit block to initialize. 

8.7:

- Unconditional jump: causes execution to jump to another spot in the code.   
  - Always happens.  
- Goto: use a statement label and go there.  
- Goto: has function scope.   
  - Label is visible throughout the function before point of declaration.  
- Statement labels must be associated with a statement, so to do nothing, need to add a null statement.  
- There’s no requirement to do a forward declaration for statement labels.  
- Limitations:  
  - Can only jump within a single function.  
  - Jump forward, can’t jump over the initialization of any variable in the scope of th elocation.   
- Should not use goto in c++, causes spaghetti code through arbitrary jumps across the code. 

8.8:

- Introduction to Loops:  
  - While statement: while condition, statement.  
  - If while statement evaluates to false initially, then the associated statement will not execute at all.   
  - Infinite loop: when the expression evaluates to true all the time.  
  - Forever infinite loop: just add a while true.  
- Don’t add a semicolon after the loop condition, results in an empty loop body that does nothing.  
- Be careful about placing a semicolon after the condition of a while statement, since it can cause an infinite loop unless we can evaluate to false.  
- Using an unsigned number as the loop variable, results in overflow.

8.9:

- Do while: looping construct where the statement always executes at a least once.   
- Can’t declare the variable used in the while conditional statement inside the do block.  
- Prefer a normal while loop.

8.10:

- For loop: basic, for (init-statement, condition, end-expression).   
- Don’t be stupid and use operator\!= for loop comparisons.  
- For (;;) → equivalent to a while loop.  
- Only define variables in the scope of the correct loop.

8.11:

- Break, lets you leave code earlier.  
- Break vs return:  
  - Break statement terminates switch or loop.  
  - Return: terminates the entire function within.   
- Continue lets you skip the iteration of the loop and go the next iteration.  
  - The end statement of a for loop is still executed after a continue.  
  - Continue works by jumping to the bottom of the loop.  
- Break and continue is acceptable by reducing the number of variables (boolean flags) being used and by keeping the number of nested blocks down as well.

8.12:

- Halt: flow control statement terminating the program.   
  - Implementing through a function rather than a keyword.  
- When the main function returns, all local variables and function parameters are destroyed.  
- A special function called std::exit() is called, with the return value from main being passed in as an argument.   
- std::exit() → causes the program to terminate normally.  
  - Cleans up things: gets rid of objects with static duration (created when program is initialized and destroyed when the program ends).  
  - This is implicitly called when main returns()  
  - Can also be called explicitly.  
  - Does not clean up local variables in the current function or the call stack.  
- Can register a clean up function at exit (std::atexit(cleanup));  
  - Pass in the function named.  
- std::abort() → unusual runtime error and the program couldn’t continue to run.  
- std:;terminate() → used in conjunction with exceptions, implicitly calls std::abort()  
- When should u use a halt:  
  - Almost never, destroying local objects is important in c++.   
  - None of the methods here destroy local objects (even though the operating system can take care of it, but that’s not ideal).  
  - Only use a halt if there’s no safe or reasonable way to return normally from the main function.

8.13:

- Introduction to random number generation.  
- Computers: generate random numbers through particular algorithms.  
- Stateful algorithm: retains information across calls.  
- Stateless algorithm: does not store any information.   
- Deterministic algorithm: for an input, it will always produce the same output.  
- PRNGs:  
  - Pseudo-random number generator: algorithm generating sequence of numbers whose properties simulate sequence of random numbers.   
- To generate different outputs, the initial state of PRNG needs to be varied (done by setting the initial state with a random seed).  
- Seed restricts the number of unique values a program can generate.   
- Need to have a high entropy seed (so a larger seed for more bits that enables states).  
- Bunch of characteristics of PRNG algorithms:  
  - High quality seed, have a long period.   
- Mersenne twister: only algorithm that ships w C++ with decent performance and quality. 

8.14:

- Generating random numbers using mersenne twister.  
- Mt19937, twister generating 32 bit unsigned integers.  
- Mt19937\_64, generates 64 bit unsigned integers.  
- Compressing the range of a random number generator into a fixed range: random number distribution.  
- Need good quality seeds? Use the system clock.

8.15:

- Some BS fade about random numbers.