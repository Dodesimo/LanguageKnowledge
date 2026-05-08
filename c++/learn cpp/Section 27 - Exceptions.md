27.1:

- Return values can be cryptic.   
- Passing in variables are also messy.  
- Error code returns don’t work with constructors.

27.2:

- Exceptions are handled through throw, try, catch.  
- Throw statement: signal that an exception or error case has occurred.   
  - Typically an error code, description of problem, or custom exception class.  
- Try block is an observer and looks for exceptions.  
- Catch block: handles the actual error.   
  - So what happens is that a try block detects exceptions and routes them to a catch block with a matching type for handling.   
- After matching catch block executes, exception resumes as normal.   
- Catch parameters are the same as a function parameter (caught by value), use const reference to prevent copies or slicing.   
- No type conversion done for exceptions: so int exception isn’t matching a double.   
- So if an exception is raised, we find the nearest enclosing try block and see if catch handlers can handle it.  
  - If so, we go to the top of the block.  
- If not, we look at subsequent enclosing try blocks  and see if there’s a catch handler.   
- There’s no implicit conversions or promotions.

27.3:

- Throw statements don’t have to be directly placed in a try block.   
- Exceptions can be thrown from anywhere in a function, and those are caught in the try block of the caller or (caller’s caller).   
  - Execution jumps from the exception thrown to the catch block.  
- So functions can delegate the responsibility for handling an exception back to its caller.   
  - This enables code to be as modular as possible.  
- When an exception is thrown, program first sees if the exception can be handled immediately within the current function (corresponds to catch block).  
  - If not, go check whether the function's caller can handle the exception.   
  - Keep doing this till there’s a matching exception handler or all functions have been exhausted.  
  - Requires unwinding the stack (removing the current function from the call stack).   
  - This means all local variables are destroyed as usual, but no value is returned.  
- Associated catch block for a function call is executed when we go up the stack.   
- As soon as an exception is caught, we go to to the code beyond a catch block.  
- When an exception gets handled, we go beyond it to the next part.   
- If a function has a matching catch block, bu call doesn’t occur within the try block, catch isn’t used.   
- Once a matching catch block executes control flow proceeds with the first statement after the last catchblock. 

27.4:

- When there’s no exception handler for a function, std::terminate() is called, application is terminated.  
  - Call stack may or may not be unwound.   
  - Cleanup expected upon destruction doesn’t happen, so if not unwound, local variables will not be destroyed.  
- Stack not getting unwind: good because we still have knowledge about the exception.  
- Catch-all handler must be placed last in the catch block chain.   
  - Ensure that exceptions can be caught by exception handlers tailored to specific data types if those xist.   
- Using catch-all handler: wrap the contents of main() just so stack unwinding occurs and local variables are destroyed.  
  - But this isn’t helpful for debug builds (we lose diagnostic information).   
  - Os use a preprocessor directive like ifndef and create a dummy class of an exception that gets compiled in so we still maintain requirement that a try always has a catch.

27.5:

- Exceptions are useful in member functions.   
  - Such as when checking the validity of an index.  
- Also useful for constructors (show that the object failed to create).  
  - But the destructor never executes.  
- What happens when resources have bee allocated in constructor and exception occurs prior to constructor finishing.  
  - You can wrap anything that can fail in a try block, and then do the destruction in a corresponding catch block for cleanup.   
- Exception classes: normal class designed specifically to be thrown as an exception.   
  - We design a class for a particular error.  
  - Note that exception handlers should catch class exceptions through reference instead of value (prevents compiler from making expensive copies).   
- Derived classes will also match base classes.   
- Have most specific classes at the top.   
- Std has a variety of exception classes.   
- Noexcept keyword: function doesn’t throw exceptions itself.  
- Lifetime of exception: object thrown in a temporary or local variable allocated on the stack.  
  - How does the exception survive the unwinding?  
- When an exception is thrown, the compiler copies the object to some piece of unspecified memory.    
  - Means that the object thrown needs to be copyable.

27.6:

- Situation where you don’t want to fully handle the issue where you catch it.   
- You can throw an exception from a catch block.  
  - Exceptions thrown within a try block are eligible to be caught.   
- If an exception is thrown in a catch block, it will be propagated up the stack.   
- Can also rethrow the same exception?  
  - By literally throwing exception, but this could be less performant.   
  - Can also be sliced if we do a catch.  
- Best way: just do throw with no variable.  
  - Propagate it up the call stack.

27.7:

- Constructor of one class calls the constructor of the other.   
  - Something wrong could happen in construction.  
  - We don’t want to catch in main program.  
- You can wrap a function in a try instead of a block of code.   
  - This rethrows the error which propagates up the call stack.  
- A function level catch block for a constructor can’t resolve exceptions.  
- Only a function level catch block for a destructor can resolve the current exception through a return statement.   
- Function level catch block for a destructor can throw, rethrow or resolve.   
  - End of the block: implicit rethrow.   
- For a normal function, the end of the block implicitly resolves it. 

27.8:

- Exception dangers and downsides:  
  - How to deal with resources not being freed?  
  - After the catch, close the file.   
  - For pointers, make them a smart pointer so they are automatically delalocated when goes out of scope.   
- Don’t use an exception in a destructor.   
  - Causes issues with stack unwinding.  
- When are exceptions valid?  
  - Error handling is going to happen infrequently, serious, execution cannot continue otherwise. No alternative way to return back to caller. 

27.9:

- Exception specifications: language mechanism that documents what exceptions a function may throw.   
  - Non-throwing function: promises to not throw exceptions.  
- Noexcept: the function can catch and handle exceptions internally as long as they don’t exit the function.  
  - Even if there’s an exception handler that could handle this up the stack.  
  - If it doesn’t, we do std::terminate, resulting in a lack of stack unwinding.   
- Functions differing in exception specification cannot be overloaded  
- What functions are implicitly nonthrowing:  
  - Destructor.  
- Non-throwing by default for implicitly declared or default functions:  
  - Constructors, assignments, comparison operators.   
- However, if any call another function that is potentially throwing, listed function is treated as potentially throwing.   
- Potentially throwing if not implicitly declared or defaulted:  
  - Normal functions  
  - User-defined constructors  
  - User-defined operators.   
- Exception safety guarantee: contractual guideline about how functions or classes behave in the event of an exception.   
  - Four levels:  
    - No guarantee: no guarantee about what happens  
    - Basic guarantee: if thrown, no memory leaked and the object is usable. But program maybe in a modifiable state.  
    - Strong guarantee: exception thrown, no memory leaked program state doesn’t change, function must completely succeed or have no side effects (achieved by failure happening before modifications or by rolling back changes).   
    - No throw/no fail guarantee: function always succeeds or fails without throwing an exception that is exposed to the caller (no-throw) (exception may be internal).   
- No throw: function fails, won’t throw an exception (returns an error code or ignores the problem), required during stack unwinding (because confusion between going up call stack and destroying)  
  - Should be destructors and memory deallocation/cleanup functions.  
  - Functions that higher level no throw functions call.  
- No fail: function should always be successful (stronger version of no throw)   
  - Move constructors move assignments, swaps, clear/erase/reset, operations on std;:unique ptr, functions higher level no fails need to call.   
- Always mark move constructors, move assignment, and swap noexcept.   
- Consider marking functions that have a no-throw, no-fail guarantee, copy constructors and copy assignment operators to take advantage of optimizations.  
  - Destructors as well (even though they are implicitly no except). 

27.10:

- Std::move → cast an l value to an r value and do move semantics.   
- Strong exception guarantee: function interrupted by an exception, no memory leak and program state doesn't change.   
- When we copy, object getting copied isn’t getting harmed since its not getting modified.  
  - We can disregard the failed copy.  
- Move:  
  - Resource going from source to destination object.   
  - If it goes wrong half way, the source object is left in a modified state, but if this is non-temporary, this damages it.  
  - Doesn’t agree with strong exception guarantee since program state changes (no guarantee a reversion works).   
- Noexcept move constructor would be ideal here.   
- Std::move\_if\_noexcept: if the object passed in won’t throw an error if you move constructor, then it will perform identically to std:;move, otherwise return a normal l value reference.   
  - With a normal l value reference, copy constructor is used.  
- If a type has both potentially move semantics and deleted copy semantics, move if no except goes ahead and invokes move semantics.