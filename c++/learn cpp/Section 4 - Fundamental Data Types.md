## 4.1

- C++ is byte-sized chunking.
- Data type: how to interpret sequences of bits. 
- Bunch of different data types

## 4.2

- Void: no type
- Incomplete type: declared but not yet defined 
- Thus cannot be instantiated.
- Indicates function does not return a value.
- Can also pass in void as a parameter to show that a function does not take any parameters. 
- Also used for void pointers.

## 4.3

- Object sizes and size of operator. 
- Most objects take up more than one 1 byte of memory. 
- When we access variable names, the compiler can hide the details of how many bytes an object uses. 
- Access some variable, compiler knows how much data needs to retrieved based on the type of the variable. 
- C++ doesn’t define exact size of any fundamental types.
- Object must occupy at least one byte. 
- Byte must be at least 8 bits. 
- Char short int long long long minimum of 8, 16, 16, 32, 64.
- char and char8_t is exactly 1 byte. 
- Boolean:
- Bool: 1 byte
- Character: 1 byte, 8, 16, 32, represents bits. 
- Short, int, long, long long → 2, 2, 4, 8
- Float, double, long double → 4, 8, 8 bytes
- Pointer: std::null_ptr: 4 bytes minimum
- Can use sizeof to determine size of particular data types.
- Cannot use sizeof on a void. 
- Does not work on dynamically allocated memory. 

## 4.4

- Integer: integral type representing positive and negative numbers. 
- Four:
- Short int (minimum of 16 bits) (just declare as short)
- Int (minimum of 16 bits) 
- Long int (32 bits) (just long)
- Long long int (64 bits) (just long long)
- C++ only guarantees that integers have a certain minimum size.
- Signed integers: numbers sign is stored in a bit.
- Can have a signed in front of the integer, but this is redundant. 
- Signed integer of size n bits: -2^(n - 1) to 2^(n - 1) - 1.
- Two’s complement. 
- If during evaluation of an expression, result is not mathematically defined or in the range of representable values, behavior is considered undefined. 
- Known as overflow. 
- Arithmetic operation creates a value outside the range that can represented called integer overflow 
- Integer division results in truncation. 

## 4.5

- Unsigned integers: can only hold non-negative whole numbers. 
- Add an unsigned keyword in front of the term. 
- Unsigned integer: 0 to 2^(n) - 1. 
- Overflow occurs when we try to store a number that’s greater than range of bits. 
- Divide the out of range value by one greater than one larger than the number of the type, and keep the remainder.
- Or just mod 2^(n) - 1. 
- Should avoid unsigned integers:
- Signed values: can cause accidental overflow at the top or bottom of the range. 
- Unwanted wrap around happens.
- Unexpected behavior can occur when signed and unsigned integers are mixed; in C++, if an operation has a signed integer and an unsigned integer, signed integer gets converted to an unsigned integer. The result becomes unsigned. 
- Ultimately, favor signed numbers over unsigned numbers for holding quantities, and avoid mixing signed and unsigned numbers. 
- What are some cases where unsigned numbers are better?
- When dealing with bit manipulation. 
- Or when wrap-around is unavoidable. 

## 4.6

- Cannot be optimistic and assume that the size of an integer is greater than the minimum. 
- Fixed width integers: guaranteed to be the same size.
- Std:int8_t and std::u_int8_t → behave like chars 
- Fixed width integers are aliases for existing integral types with a desired size. 
- In most cases int 8_t is an alias for signed char. 
- Programs fail to compile in situations where there are integral types that don’t match a particular binary representation. 
- Or sometimes fixed-width may not be the fastest. 
- Solution: 
- Fast and least integral types:
- Fast: gives you the fastest signed integer type thats at least x bits (std::int_fast_#_t)
- Least: provides the smallest signed/unsigned integer type with a width of at least x bits (std::int_least_#_t). 
- Int_fast16_t: actually 32 bits and int_least16_t; this is because 32 bits are faster to process tah n 16 bit integers. 
- We should avoid fast and least integral types because they exhibit different behaviors on architectures resolving to different sizes. 
- Prefer int when the size of the integer doesn’t matter. Prefer std::int#_t, when the quantity needs a guaranteed range. 
- Prefer std::uint#_t when doing bit manipulation or well-defined wrap around behavior. 
- Avoid short and long (prefer a fixed-width integer type)
- Fast and least integral types )prefer a fixed-width integer type). 
- Sizeof returns a value of type std::size_t.. 
- This is an alias for an implementation-defined unsigned integral type. 
- This is how all size of objects is measured in a system.
- Guaranteed to be unsigned and at least 16 bits. But on most systems, its equivalent to the address width of application. 
- For 32 bit applications, it will be a 32 bit unsigned integer. 
- Therefore, the byte-size of an object cannot be larger than the largest value that std::size_t can hold. 

## 4.7

- Scientific notation, just know the basics.

## 4.8

- Floating point: hold a fractional component of an number. 
- Can support a variable number of digits before and after the decimal point. 
- Float (single precision), double (double precision), and long double (extended precision)
- Single precision: 32 bits, 1 bit sign, 8 bit exponent, 23 bit mantissa (4 bytes)
- Double precision: 64: 1 bit sign, 11 bit exponent, 52 bit mantissa (8 bytes)
- Long double: different platforms varies between 8 and 16 bytes, and may not have a IEEE 754 compliant format. 
- By default std::cout doesn’t print the fractional part of a number if the fractional part is 0. 
- Can print number in scientific notation.
- Infinite precision number would require infinite memory to store, but we only have 4 to 8 bytes per value. 
- Results in some imprecision. 
- Precision: how many significant digits it can represent without information loss. 
- Depends on both the size and particular value. 
- For example, a float has 6 to 9 digits of precision, can exactly represent up to 6, 7 to 9 may or may not be represented exactly depending on the specific value. 
- Std:cout has a default precision of 6. 
- Can sometimes switch to outputting numbers in scientific notation. 
- Can alter default precision through an output manipulator called std:;setprecision(). 
- Favor double over float unless we care about space because we need precision. 
- Double summing is imprecise. 
- Inf, Nan: special numbers. 

## 4.9

- bool data type, true and false. 
- ! → negation operator. 
- Booleans are stored as integral values: 1 for true and false is stored as 0. 
- Can initialize a boolean with 0 or 1 (0 means false, 1 is true).
- For other integers, if doing brace initialization since narrowing isn’t allowed, this returns an error. 
- For copy initialization, there’s narrowing, so any integer not 0 gets evaluated as true. 
- Else False.
- Std:cin: only accepts numeric input for boolean variables (only 0s and 1s). Any other other numerical value will be interpreted as true and cause std:cin to enter failure. Any other value causes false
- You want to extract to std::boolalpha. 

## 4.10

- If statements:
- If (condition) true_statement.
- If (condition)
- True_statement.
- If else if else if else, not like in python where its elif. 
- All non-zero values get converted into boolean trues. 
- If (x) → if x is non-zero/non-empty. 
- You can return early from a function. 

## 4.11

- Can initialize chars with integers, but should be avoided.
- Buffer normally ignores leading white space. 
- std::cin.get() → considers white space. 
- Char is defined to be a 1 byt ein size. 
- ‘\n’ → new line, ‘\t’ → new tab.
- Single characters: single quoted (use ‘ to define a char).
- Multicharacter literals: char literals that have multiple characters. Avoid because tis compiler dependent. 
- Char16_t, char32_t were created to provide support for 16 bit and 32 bit characters

## 4.12

- Type conversion:
- Implicit type conversion: when the compiler does the conversion on behalf of us. 
- Does not alter the original value at all. 
- Instead, uses a temporary object.
- Unsafe type conversions: 
- A narrowing.
- Brace initialization ensures that we aren’t being stupid. 
- Explicit type conversion: tell the compiler to convert a value from one type of object to another. 
- We tell the compiler this is an explicit cast. 
- static_cast<new_type>expression
- Static cast does not impact the variable itself. 
- Static cast can also be used to convert between signed versions if both are within the range.
- If it cannot be within the range, if destination type, value will be modulo wrapped.
- If the destination type is signed, the value is implementation-dependent.
- Can validate that std::int8_t actually outputs itself as a integer through a static_cast<int> on that number. 
- Causes issues when inputting. When asking input in the form of int8_t, each number that may be two digits gets represented as its own character. 