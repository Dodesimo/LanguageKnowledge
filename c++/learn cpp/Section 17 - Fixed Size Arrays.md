17.1:

- Container: store collection of unnamed objects.  
- Arrays: allocate their elements contiguously in memory.   
- Fixed-size arrays: require that length of array is known at point of instantiation.  
- Dynamic arrays: resized at runtime, std::vector is a dynamic array.  
- Why is dynamic arrays not the wave?  
  - Std::vector is slightly less performant than fixed-size arrays.  
  - Doesn’t support constexpr.  
- Std::array → constexpr, std::vector → non-constexpr arrays.  
-  Std::array length needs to be a constant expression.  
- Std::array can have no data.   
- Std::array is an aggregate, doesn’t have a constructor, initialized with aggregate initialization.  
- Less items provided in initializer list than the array length, we value initialize them.  
- Std::array can be const.  
- Std::array has full support for constexpr.   
- CTAD allows you to deduce element type and array length from a list of initializer.  
  - CTAD doesn’t support partial omission of template arguments.  
- You can omit array length using std::to\_array.  
  - But this is expensive because it creates a copy of a temporary std::array.

17.2:

- Std:;array, also uses unsigned values for lengths and indices.  
- Again, sign conversions are narrowing conversions except when we have constexpr.  
  - The length of a std::array is of type std::size\_t.  
  - Because of this, we don’t run into sign conversion issues when we use a signed integral value, since the compiler will happily convert a signed integral value to a std::size\_t at compile time without doing a narrowing conversion.  
- Like all container classes, std::array has a nested typedef size\_type, which is an alias for the type used for the length and indices of a container.  
  - Size\_type is always an alias for std::size\_t.  
- Unlike std::string and std::string\_view, which have both length and seize member function, std::array only has seize()  
- std::ssize() → returns the array size as a large signed integral type.  
- std::size() is always a constexpr value.  
  - However, when they are called on a std::array function parameter that’s passed by const reference.  
- You can subscript std::array using operator\[\] or at() member function.  
  - But at() is not as efficient, and we want bounds checking before an index or want compile-time bounds.  
- Function parameters cannot be constexpr because their value is not known at the time of the function definition.   
- To get compile-time bounds checking, we can use std::get with an integer template argument that is the index of the std::array we want to print.   
- Template arguments must be constexpr.  
  - Program needs to have the correct functions produced for different template types at compile time. 

17.3:

- Passing and returning std::array  
- Can be passed into a function like any other object.  
- Pass it by const reference.  
- Need to explicitly specify the element type and array length.  
- Templates don’t do conversions.  
- Can use auto for non-type template parameters.  
- Unlike std::vector, std::array does not have move semantics.  
- You can return a std::array by value if the array is not huge, the element type is cheap to copy or mode, or the code isn’t being used in a performance sensitive context.  
- If you don’t want a copy, pass in the array as an out parameter.  
- Or just return std::vecror because you are probably not returning a constexpr std:array.

17.4:

- Std::array isn’t just limited to elements of fundamental types.  
- When initializing a struct, can’t use CTAD on the actual struct inside the container.  
- Why?  
  - std::array : is a struct that contains a single C-style array member.   
  - So when we provide a series of initializer lists, the first list populates the element at the 0th index, rest of the members are 0-initialized.  
  - However, if we wrap all three arrays with an extra set of braces.  
  - This is because of aggregate initialization  
- Brace elision: when can you omit multiple braces?  
  - When initalizing with single values or class types arrays where the type is explicitly named with each element.  
- Don’t need double braces when you mention the type for each item.

17.5:

- Because references are not objects, you can't make an array out of them.  
- But, you can use std::reference\_wrapper that serves as an array of references.  
  - .get to get values and change them.  
- Std::ref deduces to std::reference\_wrapper\<int\>  
- Std::cref deduces to std::reference\_wrapper\<const int\>

17.6:

- If there are less initializers than provided, array will be shorter than expected, indexing leads to undefined behavior.   
- Sv suffix: forces inference as std::string\_view.   
- You can make an array of type Color::Type, and then iterate over that.

17.7:

- C-style arrays, inherited from C language.  
- Own declaration syntax, square brackets tell the compiler an object is C-style larray, can optionally provide length of array.  
- C-style arrays must at least be 1\.  
  - Unless dynamically allocated on the heap.  
- Length must be a constant expression, the indices can be of any integral type.  
-  Unlike standard library container class, index of a C-style array is a value of any integral type (signed or unsigned or an unscoped enumeration), no sort of sign conversion indexing issues.   
  - When doing an array, the use of \[\] is part of the declaration syntax.  
- C-style array is an aggregate, so you can do aggregate initialization through direct initializer list.  
- More initializers, compiler will error.  
  - Less, remaining elements are value initailized.  
- CTAD and auto don’t work on arrays.  
- Const: must be initialized and cannot be changed after.  
- Sizeof returns the total size of the array.   
- C++ arrays don’t support assignment 

17.8:

- Passing in an array.   
- In most cases, when C-style array is used in expression, array is implicitly converted to the pointer of the the first element.   
  - The index of the first element.  
- Const array: decays into pointer-to-const.   
- Where does C-style array not decay?  
  - When used for sizeof or typeid  
  - Taking address of array using &.   
  - Passed as a member rof a class type  
  - Passed by reference.   
- Array type has length information.  
- Decayred array type is an int\*.  
- Subscritping an array applies \[\] to the decayed pointer.   
- Because C-style arrays are passed by address, copies of the arrays are not made.   
- Two arrays with the same element type but different length will decay into same pointer type.  
- Issues with array decay:  
  - sizeof() returns size for arrays, pointer size for decayed array.  
  - std::size() and std::ssize() fails to compile when a pointer is passed.  
  - Hard to get access to length.   
- Solution:  
  - Can pass in both array and array length as separate arguments (but hella responsibility)  
  - Add a sentinel value at the end that can be used to mark the end of the array.  
- Avoid C-style arrays.  
- Unless, we are storing constexpr global program data 

17.9:

-  Pointer arithmetic: apply certain operations to a pointer to get a new memory address.  
- So if you add 1, you will get the starting address of the next object based on the type.  
- Pointer arithmetic and subscripting are relative addresses.  
- For a pointer at a C-style array element, pointer arithmetic is valid as long as the resulting address is a valid array element.  
  - Begin and end pattern: what iterators use.

17.10:

- C-style strings: have a length thats one more than the actual number of characters (1 hidden null-terminator character).  
- C-style string variable, declare a C-style array (char, const char, constexpr char).   
  - Compiler takes care of null character termination.  
- C-style string literals decay into const char\*.  
  - String arrays decay into either a const char \* or char \* depending on whether the array is const?  
- Decay: gets rid of length of string.  
- Std::cout will output characters until it runs into null terminator.  
  - So if a string doesn’t have a null terminator, the result will be undefined behavior. Keep reading from memory till an empty slot is reached.  
- Array overflow or buffer overflow: security issue when more data is copied into storage than what the storage can hold.  
- You can initialize a C-style string upon creation, but can’t assign values to it using assignment after it.  
  - But you can change individual characters.  
- Std::size and std::ssize can be used to get the length of a string as an array.  
  - Doesn’t work on decayed strings  
  - Returns the actual length of the array (not the length of the string).


17.11:

- Difference between const char (const C-style string initialized with C-style string literal), and const char\* const (const pointer to a C-style string literal)  
- Const char.  
  - Put onto read-only memory.  
- Const char\* const:  
  - Implement defined, compiler places it into read only, and then gives the pointer the address of the string.   
- If you pass in a char\* or const char\*, compiler assumes you’re printing the string being potined to rather than the pointer.  
- Just use std::string\_view.

17.12:

- Dimension of an array: number of indices needed to pick an element.  
- Two dimensional arrays exist in C++;  
- How are 2d arrays laid out in memory?  
  - Memory is linear, C++ uses row-major order, elements are placed row-by-row.

17.13:

- Multidimensional std::array, use double braces (brace elision).  
- Use an alias template to replace a std::array\<std::array\<T, Col\>, Row\>;  
- Use special function to return row and column.  
- Flattening an array: reduce dimensionality of an array down to a single dimension.  
- mdspan(), use to see a two dimensional view of data.  