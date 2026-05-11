- string: variable-length sequence of characters
- vector: variable-length sequence of objects of a particular type
- Don't use `using` in headers (cause name conflicts).
- String:
	- Initializing a string:
		- ```c++
		  std::string s1; //default init
		  std::string s2 = s1; //copy
		  std::string s3 = "hiya" //copy of string literal
		  std::string s4(10, 'c'); //invoke constructor that takes count and character. 
		  ```
	- Initializing with =: copy initialize the object on the right.
	- Without an =: direct initialize.
		- Direct initialization: means invoking a matching-constructor.
	- Default initialization of a string is always empty.
	- Initializing a string from a literal does not copy over the null character (`\0`)
	- `>>` for a string takes the first value before white space.
	- Operators are returned (left-hand side), so can chain together multiple reads, writes.
		- `getline`: takes input string and string, reads up to first newline. 
		- newline is not stored in the string.
		- `endl`: flushes a buffer
	- `string::size_type`: use library types in a machine-independent manner.
		- Don't know the precise-type, but large enough to hold the size of any `string` on this system.
	- Two strings: equal if they are the same length and have the same characters.
	- Relational operators: dictionary-style comparisons.
	- Adding strings: result is a concatenation, characters are copied from left-hand followed by right-hand.
	- Mixing strings or other literal types, at least one operand to each + operand should be string type.
		- Literal concatenation is done left to right (OOP)
	- Processing characters: use range-based for.
	- ``cctype`` functions for character processing:
		- ```c++
		  isalnum(c) //if c is letter or digit
		  isalpha(c) //if c is a letter
		  isdigit(c) //if c is a digit
		  islower(c) //if c is a lowercase letter
		  ispunct(c) //if c is punctuation
		  isspace(c) //if c is a space
		  tolower/upper(c) //changes c to opposite case unless its already in that state. 
		  ```
	- Subscript operator: takes a `string::size_type` int, and gives a reference to the character at that position.
		- Indexing outside the range of a string is undefined.
	- When indexing, we should use type of `string::size_type`. 
		- Only check upper bound of value.
- C++ and C Headers:
	- C++ headers have `cname`. (put in the `std` name space)
	- Headers in C: have name in the form of `name.h`.
- Vector:
	- Collection of objects that have the same type.
	- Class template.
		- Templates: instructions to the compiler for generating classes/functions.
		- Provide information about the type of objects a vector will hold.
	- Cannot have a vector of references.
	-  Can't initialize a vector literally by putting parenthesis around all items.
	- Not specifying the value in `std::vector<string> test(10, "hi")`: all items get value initialized if built-in type.
		- If class type, element initializer is default-initialized.
		- Does not work where we can't default initialize.
		- Only can use direct form of initialization.
	- Curly braces used to disambiguate between curly braces and parenthesis.
		- Parenthesis: these values are used to construct object.
			- `std::vector<int> v1 (10,1)`: 10 elements with a value of 1.
			- `std::vector<int> {10, 1}`: two elements with value of 10 and 1.
		- Curly braces: list initialize the object.
			- If we use braces, but no way to use the initializers to list-init, values can be used to construct object.
			- `std::vector<string> v8 {10, "hi"}`, list init, but since 10 can't be a string, we go to direct initialization.
	- Defining the size of a vector at first is not efficient.
	- Cannot use range for loop to add elements to a vector.
		- Since the body of a range for can't change the size of sequence its iterating over.
	- `std::vector<int>::size_type`, always need to specify the underlying data type the vector holds.
	- Subscripting a vector doesn't add elements. 
		- Need to do `push_back()`
		- This is because subscripting on vector fetches existing element, doesn't add one.
- Iterator:
	- Iterator: indirect access to object in a container/char in string.
	- Use an iterator to fetch element and iterators have operations to move from one element to another.
	- Valid: denote an element or denote a position one past the last element.
		- `.begin()`: iterator at the beginning of the collection
		- `.end()`: iterator at one past the last element in the collection.
			- Container empty: both are the same.
	- Iterators equal if they denote same element or are both off-the-end.
	- Dereference the iterator to get the element.
		- Get a reference to that element.
	- Incrementing iterator: advance the iterator by one position. 
	- Iterator: we use `!=` because all library containers with iterators have that defined instea of equality/comparision.
	- `const_iterator`: allows only read access to the element it denotes.
	- Iterator: a type that supports a common set of actions.
		- Access element in a container, move from element to another.
	- Best practice of using a constant type for an iterator.
		- request using `cbegin()` and `cend()`, they always return a const_iterator
	- Dereference and then do a member access: wrap the dereference in parenthesis and then dot (otherwise dot refers to the iterator):
		- `(*it).empty()`.
		- Use arrow operator.
	- Operations like `push_back` that change the size of a vector potentially invalidates iterators.
	- Iterators for `string` and `vector` support additional properties like moving multiple elements/relational operations. 
		- Subtract two iterators to get the offset (what to add to the right-hand iterator to get the left).
			- Results in a type called `difference_type`.
	- Doing arithmetic: must result in an iterator that is an element or one past the end of the associated vector.
- Arrays:
	- Container of objects of a single type that is not dynamic.
	- Dimension of an array is part of type + must be a constant expression since known at compile time.
	- Elements of an array are default initialized by default.
	- Cannot use `auto` to deduce type from a list of initializers for an array.
	- No arrays of references.
	- List initializing from the elements of an array: can omit dimension.
		- Specify a dimension: # of initializers can't exceed specified size.
		- If dim > # of initalized, rest are value initialized (even in functions)
	- Character arrays: can be initialized with string literals (but need space for the null character).
	- Can't copy or assign arrays.
	- Read declarations from the inside out. 
	- Array subscripts are of `size_t`.
		- Unsigned type large enough to hold the size of any obj. in memory.
	- In most places where we use array, compiler substitutes a pointer to the first element.
		- So when using `auto`, the deduced type is a pointer.
		- This conversion doesn't happen w/ `decltype`.
	- Pointers behave the same as iterators.
		- Can get an off-the-end pointer by getting address of element one beyond the last index for the array.
		- To avoid mistakes, use function `begin()` and `end()` to get functions.
			- Not member functions b/c arrays are not class types.
			- Defined in `iterator` header.
		- Adding an integral value to a pointer:  must be a pointer to an element in the same array or a pointer just past the array.
		- Relational operator on pointers on two unrelated objects is non-sensical.
		- Null pointers:
			- Can only add or subtract constant integral expression w/ value of 0 to p.
		- `*(array + i) = array[i]`.
	- Subscripting array -> same as subscripting pointer to element in array.
		- `array[2]` --> fetching element to which (`array + 2`) points to.
		- Can subscript any pointer as long as pointer points to element or one past last element in array.
	- Library types: force index w/ subscript to be unsigned value.
	- Built in subscript doesn't, so can be negative value (resulting address must pointer to element or one past the end of array).
- C-Style Character Strings:
	- Convention for how to represent and use character strings.
	- Stored in character arrays and null terminated.
	- Need to pass in null terminated string to routines defined in `cstring` header.
		- If not null terminated, undefined.
		- Common functions:
			- `strlen(p)`: returns length of p without null
			- `strcmp(p1, p2)`: compare the two, 0 if equal, positive if p1 greater, negative otherwise.
			- `strcat(p1, p2)`: append p2 to p1, return p1.
			- `strcpy(p1, p2)`: copy p2 into p1, return p1.
	- Using relational operators on C-style strings compares pointer values themselves.
		- Meaningless if two pointers are for different strings.
	- Concatenating two C-style strings w/ `+` is also meaningless (tries to add the two pointers).
		- Use `strcat` and `strcpy`: pass in an array large enough to hold resulting string.
- Compatibility:
	- We can use a null-terminated character array anywhere there's a string literal.
		- Init./assign a string.
		- Null-terminated character array as one operand when adding to a string.
	- No way to directly use library string when C-style string is required.
	- To get C-style string representation:
		- `.c_str()`
		- Returns pointer to beginning of null-terminated character array.
		- Pointer type: `const char*`: prevents us from changing contents of array.
		- Can get invalidated by changing the original string (so copy it).
	- Can initialize a vector with an array: pass in the address of the first element and one past the last element we wish to copy.
		- Can slice part of an array.
- Multidimensional arrays:
	- Arrays of arrays.
	- `int array[rows][cols]`: dimension of array itself, and then dimension of elements.
		- Read inside-out.
	- Can be list-init by specifying bracketed values for each row.
		-  Nested braces not required: can just directly have all values in the init.
	- `int array[3][4] = {{0}, {4}, {8}}`: each row has the first element init.
	- `int array[3][4] = {{0, 4, 8}}`: first row is populated like that.
		- Remember:
			- `std::vector`, `std::string` default initialized to defaults
			- C-style strings, arrays, are not unless they are outside a function (static duration)
				- Values are default initialized when partially filled.
	- Subscripting:
		- Usual if all dimensions are provided.
		- Fewer subscripts: get the inner-array element.
			- `array[2]` --> returns the last row.
		- Can use a nested for loop to iterate and access elements.
			- Can also use a range for loop.
			- ```c++
			  for (const auto &row : rows) {
				  for (auto &col : row) {
					  std::cout << col;
				  }
			  }
			  ```
		* Why did we use a reference?
			* Without, row gets converted to pointer of first element.
			* Can't loop over that.
	* Pointer type: pointer to the first inner array.