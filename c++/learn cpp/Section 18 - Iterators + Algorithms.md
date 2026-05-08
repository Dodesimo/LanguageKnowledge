18.1:

- Swapping elements in C++, use std::swap().  
- Std::sort, allows you to sort in ascending order.  
- Need to specify a start pointer and an end pointer.

18.2:

- How do range based for loops work?  
  - They use iterators.  
- Iterator is an object designed to traverse through a container.  
  - Different types of iterators.  
  - Array containers may have a forwards iterator that goes forward direction.  
  - Array container may have a reverse iterator that walks through the array in the reverse direction.  
- Ask for the begin and end points through member functions named begin() and end().  
- When working with iterators, use \!= because some iterator types are not comparable.  
- So anything with begin() and end() member functions can be used in a range-based for loop.  
  - Doesn’t work with decayed C-style arrays.  
- When using an iterator, can’t add new elements to the end.  
  - Invalidates iterator, because all indices are moved.  
  - Undefined behavior.  
- Erase function returns iterator to element one  past the erased element.  
  - So just assign it to the iterator variable.

18.3:

- C++ standard library comes with a bunch of functions that are efficient.  
  - Inspectors: view data in a container.  
  - Mutators: modify data in a container (sorting and shuffling)  
  - Facilitators: generate a result based on values of the data members.  
- Std::find → find an element by value.  
- Std::find\_if → takes start, end, and a function pointer.   
  - Iterate ver each element, call the function, and return true if match found or false.

- Std::cout and std::count\_if searches for all occurrences of an element or element fulfilling a condition.  
-  Std::for\_each, double all the numbers in an array.

18.4:

- Chrono library → used to time code.  
- What things can impact performance of program?  
  - Using release build target instead of a debug build target (debug turns optimization off).  
  - Timing can be impacted by what system is doing in background.  
  - User input.  
- Measuring performance:  
  - Gather as many results needed to get a consistent measure of performance,
