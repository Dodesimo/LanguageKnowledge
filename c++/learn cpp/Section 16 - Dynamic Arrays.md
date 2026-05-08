16.1:

- Need some way to group a collection or container of objects.   
- Container: data type providing storage for a collection of unnamed objects.   
- The elements of a container are unnamed (can put as many in container without having to give a unique name).   
  - C++: size is the commonly used term for number of elements in a container.  
  - Prefer “length” when referring \# of elements in container, and size for amount of storage.  
- Containers are homogeneous in C++, must be of the same type.  
- C++ def of container: narrower than general, only class types in the containers are considered to be containers.  
  - What aren’t containers, C-style arrays, std:;string, std::vector bool.  
  - Close enough to C++ containers, so they are called pseudo-containers.   
- Array: stores sequence of values contiguously in memories. 

16.2:

- Std::vector: container class implementing an array.   
- Containers have a special constructor called a list constructor (allows for the creation of a container with an initializer list).   
- Use indices, offset from first element of array.  
- Indices can’t be negative unlike python  
- Arrays again are contiguous in memory.   
- Allows for random access: elements can be accessed directly.  
- Can construct a std::vector for a set size through direct initialization.   
  - This is because, {10} would cause ambiguity, and C++ would pick a matching list constructor.  
- So when constructing class type object using initializer list:   
  -  If list empty, we use default constructor  
  - Else, matching list constructor (takes precedence).  
- Can make a std::vector const.  
- Cannot make a std::vector const expr. 

16.3:

- Data type for subscribing array should match data type for storing length of the array.   
- Unsigned values are used for the length and indices.   
- Sign conversions are narrowing conversions, throwing errors or warnings.   
  - However, constexpr disregards sign conversion as a narrowing because the compiler can guarantee conversion is safe.   
- Length and indices of std::vector has type of size\_type.   
- Every standard library container class defines a nested type def member called size\_type (alias for the type used for the length).  
- Std::size\_t is the type used to represent the theoretical max size an object could be in memory?  
- Std::string and std::string\_view have both a length and size member function.  
- Std::vector only has size().  
- std::size() → non member function that can also be used.  
- Static cast the size to the variable you want to store.  
- C++20, introduces std::ssize(), returns length as a large integral type.  
- .at() does bounds checking.  
  - This is slower because ther’s a runtime bounds check every call.  
- Indexing std::vector with a constexpr signed int, compiler can implicitly convert this to a std::size\_t without a narrowing conversion.  
  - So essentially, every index gets converted to a std::size\_t.  
- Std::size\_t is an unsigned quantity.

16.4:

- Pass std::vector by const reference to avoid making copies.  
  - We have to explicitly specify element type when we use a std::vector.  
  - Again, CTAD can’t be used to infer function parameters.  
- Remember that auto parameter creates an abbreviated function template.  
- st::vector::size() is a non-constexpr function, so we can only do a runtime assert.

16.5:

- Okay to return a std::vector by value.  
- Copy semantics: rules that determine how copies of objects are made.  
- When a type supports copy semantics, we mean the objects are copyable.  
- For classes, this is implemented through copy constructor and copy assignment operator.  
  - Each data member is getting copied over.  
- Reminder that when we return from a function, we are return an rvalue temporary.  
  - When we initialize based on a temporary, can we just move the ownership of the data?  
  - This is a lot easier, just two or three pointer assignments.  
- Move semantics: rules that determine how data is moved.  
  - So, when we invoke move semantics, any data member that can be moved is moved.  
  - When any data member that can’t be moved is copied.  
- When does move semantics get invoked over copy semantics?  
  - Type of object supports move semantics.  
  - Object is initialized with an rvalue (temporary)  
  - Move isn’t elided.  
- So with move semantics, a return by value can be moved instead of copied (return by values is thus inexpensive).  
- Expensive to copy types should not be passed by value, but if they are move-capable can be returned by value.  
- When passed values are class types, there’s four steps:  
  - Construct value to be passed.  
  - Pass the value to the function.  
  - Construct the value to be returned  
  - Pass the return value back to the caller.  
- So more specifically,  
  - Initializer list copy, passing the value into function, constructing the value to be returned, returning the value copies v3 back to the caller.  
- For move-capable types we prefer to passt by const reference and return by value. 

16.6:

- Basic traversal and iteration.  
- Because the container classes use size\_t for length and indices, i think using auto makes it a lot easier to work with?  
- The rest of this fairly common sense, off by one errors, make sure you have the right bounding for arrays.

16.7:

- Using a nonnegative number causes wrap around.  
  - So a loop would never terminate.  
- Using static cast → hard to read\!  
- Solutions:  
  - Leave signed/unsigned conversion off.  
    - Not a good solution because it also suppresses actual problems.  
  - Use an unsigned loop variable.  
    - Use a size\_type for the container class (because that’s the type used for array lengths and indices).   
    - Any name that depends on a type containing a template parameter is called a dependent name (prefixed with the typename in order to be used as a type).  
    - Have compiler fetch type of array, use decltype keyword, returning type of the parameter.  
      - Doesn’t work if arr is a reference type.  
    - Fade all this and just do std::size\_t.  
  - Used a signed loop variable.  
    - Use int: default integral type.  
    - Or use std::ptrdiff\_t.  
    - Getting the length of an array as a signed value, static\_cast the return of size.  
    - Use std::ssize(), returns the size of an array as a signed type  
    -   
    - Index the underlying c-style array with a data() member function.

16.8:

- Range-based for loop: you can traverse without doing explicit indexing.  
- Use auto to do deduction of the type of array elements, no chance of accidentally mistyping.  
- Since each element in the array is copied into the variable used to iterate, use const reference.  
  - Auto → cheap to make a copy  
  - auto& → reference we want to alter.  
  - Const auto& → don’t want to make a copy and just want to view it.  
- Getting index of the current element: just use a counter.  
- You can make a reverse view for doing reverse.

16.9:

- You can use an unscoped enum because they convert to integral types.  
- Count enumerator: value represents the count of previously defined enumerators.  
  - Add an enumeration value at the end that is always updated with what the number of enum items is.  
  - Can be used to assert size of std::vector.  
- When using a scoped enum, just static cast the enumeration to an integer.,

16.10:

- Std::vector → can get resized after it has been instantiated.  
- Std::array and C style arrays are fixed-size arrays or fixed-length arrays.  
- On the other hand, std::vector is dynamic, can get resized.  
- You can also make a std::vector smaller.  
- Capacity of a std::vector: how many elements is allocated storage for.  
- Length: how many elements are being used.  
  - So a std::vector with a capacity of five has space for five elements, if the vector has two elements in active use, length of the vector is 2\.  
- What happens during reallocation:  
  - Std::vector gets new memory with capacity for desired \# of elements.   
  - Elements in the old memory are copied into new memory. Give the old memory back.  
  - Set the capacity and length of the std::vector to the new values.  
  - This is a reallocation, we want to avoid unnecessary reallocations.  
- Valid indices for a vector are based on length, not capacity.  
  - If a vector v has memory allocated to store 5 things, has a length of 3\.  
- Resizing vector to be larger: increases the vector’s length and increases capacity.  
- Resizing vector to be smaller: decreases length and not capacity.  
  - To also decrease capacity to match length, std::vector has a shrink to fit.

16.11:

- Push: add a new element on top of stack  
- Pop: remove top element from stack  
- Top or peek: get the top element on stack, empty see if the stack has no elements, size: count how many elements are on the stack.  
- This is basic stack stuff, data structure concept.  
- Stack behavior:  
  - push\_back() → push to the top of the stack  
  - pop\_back() → remove from the top of the stack  
  - back() → get the top element  
  - emplace\_back() → push (this is more efficient at times)  
  - empty(), see if the stack size is 0\.  
- Pushback increments length of vector (will cause a reallocation if capacity isn’t sufficient)  
- If you init a vector’s capacity with a number, we create a bunch of 0 values at the beginning.   
- We want to change capacity without changing length without adding new elements to the stack.  
- Reserve: allocate a std::vector without changing current length.  
  - So: resize() → changes length of the vector and the capacity if necessary.  
  - Reserve() → member function changes just the capacity (use with stack)  
- When creating a temporary object, emplace back is more efficient.  
  - This is because when making temporary object, forwards arguments so that the object is made in the vector (no copy).  
- Push back doesn’t use explicit constructors.   
- If we don’t know how many elements will be added to std::vector, use stack functions to insert those.

16.12:

- Std::bitset → has bit manipulation member functions  
  - Can compact 8 boolean values to a byte.  
- std::vector\<bool\> → lacks bit manipulation member functions  
  - Compacts 8 boolean values into a byte.  
- Trade offs:  
  - Std::vector bool has high amount of overhead (40 bytes).  
  - std::vector\<bool\> is not a vector, nor does it hold bool values, nor does it meet C++’s definition of a container.  
- We should avoid std::vector bool (incompatible, not a real container, just use std::dynamoc bitset)  
- Std::vector can be made const but not constexpr because dynamic memory allocation cannot happen in like a compile-time setting.