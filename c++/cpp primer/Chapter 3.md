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