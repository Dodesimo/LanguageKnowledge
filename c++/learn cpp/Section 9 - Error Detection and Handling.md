9.1:

- Chances are your code is intended to be ran many times with loops, conditional logic, and other user input.  
- May have had scope creep.  
- You don’t know all of this for sure, so requires software testing/validation.  
- Ideal testing mechanism:  
  - Build or buy and test each car component individually before installation.  
  - Once the component has been proven to work, integrate into the car and retest to make sure it was done correctly.  
  - Idea of unit tests and integration tests.  
- Can manually test the code.  
- Can also automate this process and compare the actual results to the expected results through a comparison.   
- Unit testing frameworks.  
- Integration testing: once unit tests have been in isolation, can be added to your program and need to be retested. 

9.2:

- Code coverage: how much of the source code has been executed for a program while testing.  
- Statement coverage: percentage of statements in your code exercised by your test routines.  
  - Sheer number of lines of code being executed.  
- Branch coverage: percentage of branches that has been executed, with each possible branch getting counted separately.  
  - The number of possible paths the code can take is getting tested.  
  - Aim for a 100% branch coverage.  
- Loop coverage: does the loop work for 0 iterations, 1 iteration, 2 iterations  
- Check a variety of different inputs. 

9.3:

- The more arithmetic done with a floating point number, more small rounding errors accumulate.   
- Can’t mix up equality and assignment operators:  
  - Assignment operator returns left operand. So , we would assign, and then return that. Since C is non-zero, it is converted to the boolean value of true.   
- Using a function name without calling it yields a function pointe rholding the address of the function. 

9.4:

- Most coding errors are assumption related errors.   
  - They test the happy path.   
  - We should also test sad paths or unideal paths.  
- What to do when we have an error?  
  - First: recover error within the function (keep retrying).   
  - Second, pass the error back to the caller. You can do this by changing the function return type to a boolean where true indicates success, false indicates failure.   
  - Can also have a sentinel value that has a special meaning in the context of a function or algorithm.   
  - Fatal error: nonrecoverable, terminate the program.   
    - If main, return a non-zero status code.  
    - If in some nested subfunction, use a halt statement.   
  - Lastly, consider passing errors back to the caller through exceptions.   
    - When error happens, exception is thrown.  
    - If the current function doesn’t catch it, the caller of the function catches it.   
    - If the caller does not catch it, the caller’s caller catches it.  
    - Moves up the stack.   
- Types of applications;  
  - Interactive applications: ones the user interacts with after running.  
  - Non-interactive applications: applications that don’t require user interaction to operate.   
    - Tools: launched to produce an immediate result, and then terminate after.   
    - Services: non-interactive applications running silently in the background to perform some ongoing function.   
- Rule of thumb for error handling:  
  - Use std::cout for user-facing text.  
  - Interactive programs: use std::cout for normal user-facing error messages and then std::cerr or a logfile for status and diagnostic information.   
  - For non-interactive programs: use std::cerr to output error, for all transaction applications, use a logfile. 

9.5:

- Robus: handles error cases well.  
- How does \>\> (the extraction operator):  
  - Leading whitespaces are discarded from the input buffer.  
  - If the input buffer is empty, we wait for the user to enter more data.  
  - \>\> then extracts as many consecutive characters as it can until it encounters either a new line character or an invalid character.   
  - If no characters could be extracted, the object being extracted to is assigned a value of 0\.   
- Process of checking if user input conforms to what the prgram is expecting: input validation.  
- Three basic ways:  
  - Inline: prevent the user from typing invalid input in the first place.  
- Post-etnry: user can enter whatever they want, validate whether that is correct, and if so, make that the final variable.   
- Std::cin does not support the inline validation mechanism.   
- To prevent unnecssary buffer hecks, we can std::cin.ignore(std::numeric\_limits\<std:;streamsize\>:;max(‘\\n’);  
  - Eseentialy ignore all remaining max stream size \# of characters on the buffer including the “\\”  
- If an extraction fails, we need to detect that it has failed, put std::cin back into normal operation mode, and remove the input that caused a failure.   
- We can do std::cin to validate whether extraction happened, and if so we clear the buffer and put it back in normal operation mode, and we remove the bad input.   
- Checking for EOF: no more data available (like when you’re reading a file).   
  - But when getting input from the user, if the user enters a special key combination for their OS, we can return a “EOF character.”  
- When extracting data from std::cin and the user enters EOF, OS-specific behavior.  
  - If EOF not the first character of input: everything prior the EOF is flushed.   
  - If EOF is the first character entered, EOF error is set.   
- So we should have an explicit check for std::cin.eof().  
- If we overflow a variable, std::cin goes into failure mode, but assigns the closes in-range value to the variable.  
  - So if x is a 16-bit signed integer, it is assigned the value of 32767 (2^15 \- 1). 

9.6:

- If there’s an invalid input, you can halt the program or skip offending statements, but these are problematic options because we are essentially failing silently.  
- Std::exit graceful exit, std::abort something seriously wrong happened.  
- Precondition: a condition that must be true before the execution of some code.  
- Bouncer pattern: bounce out of the function if you get an invalid case.   
- Invariant: condition that must be true while some section of code is running.  
- Postcondition: something that must be true after the execution of some section of code.  
- Assertion: expression that will be true unless there’s a bug in the program. If the expression evaluates to true, we don’t do anything. But if the expression evaluates to false, we display an error message and the program is terminated.   
- Why is assert good?  
  - Allows for code checks to be done.  
- You can AND a variable with a string literal and that string literal is also returned in whatever failure.   
- How to turn off asserts in production code, use the preprocess macro NDEBUG.   
- Static\_assert: a compile time check rather than a runtime check.   
  - Failing static\_assert: causes a compile error.   
  - Condition must be a constant expression.  
- Favor static assert over assert because of optimization?  
- Assertions used to detect programming errors during development.   
- Error handling: used to gracefully handle cases that could happen.   
- Buffer is a piece of memory that’s set aside for storing data temporarily when its moved from one place to another. 