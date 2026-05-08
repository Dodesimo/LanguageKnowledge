10.1:

- Conversions don’t change the data being converted.  
  - This is because when converting, the process produces a temporary object.  
- Bits are fundamentally difference between data types.  
- Implicit type conversion or automatic type conversion: performed automatically by the compiler when expression is supplied in a context where some other type is expected.  
  - When does this happen?  
    - Initializing a variable with a different data type.  
    - Type of return value is different from the function’s declared return type.  
    - Certain binary operators with operands of differen types.,  
    - When argument is passed to a function that’s different than the actual parameter type.  
- Numeric promotions: conversions of small integral types to int or unsigned int \+ promotion of floats to doubles.  
- Numeric conversions: other integral or floating point conversions that are not promotions   
- Qualification conversions: conversions removing or adding const or volatile.   
- Value transformation: conversions that change the value category of an expression.  
- Pointer conversions: conversions from std::nullptr to other pointer types.  
- Type conversions can fail if there’s no standard way of going from one data type to another.  
  - For example, casting an integer to a string.  
  - Brace initialization of a decimal to an integer (this is because brace initialization does not allow conversions resulting in data loss). 

10.2:

- Floating-point integral promotion.  
- C++ has minimum size guarantees for each of the fundamental types, but no actual size references.  
- Variability allowed so that int and double could be set to the size that maximizes performance.   
  - \# of bits that a data type uses: width  
- Some processors are better at manipulating 8 bit or 16 bit values directly, but is often slower than manipulating 32 bit values.  
- C++ doesn’t assume that CPU can efficiently manipulate values narrower than the natural data size for that CPU.  
  - Numeric promotions: type conversion of narrower types to wider types (like if we go from 8 bits to like 16 bits)  
- Promotions: allow you to pass in a smaller data type as a parameter to something that expects larger? Would require an overloaded function otherwise.  
- Floating point numerical promotion: a float can be converted to a double.  
- Integral promotion:  
  - Signed char or signed short → int  
  - Unsigned char, char8\_t, unsigned short → converted to int if in range.  
  - Bool → int.  
  - Things to note:  
    - Some unsigned integral types can be promoted to unsigned int instead of int.  
    - Narrower unsigned types can be promoted to larger signed types.   
    - Integral promotion is value-preserving, but does not necessarily preserve the signedness of the type.  
- Not all widening conversions are numeric promotions (like char to short or int to long), because these do not assist in the goal of converting smaller to larger types.

10.3:

- Numeric conversions.  
  - Earlier covered numeric promotions, conversion of narrower numeric types to wider numeric types.  
  - Converting an integral type to any other type (like int → short, int → long, short → char, int → unsigned int)  
  - Double → float, double → long double  
  - converting floating point to integral type.  
  - Integral type to floating point.  
  - Converting integral type or floating type to a boolean.  
- Numeric promotions are always value preserving, while many numeric conversions are unsafe.   
  - Value-preserving conversions: safe numeric conversions where the destination type can represent all possible values in the source type.  
    - Int to long and short to double.  
    - No warnings, completely valid.  
    - You can convert back to the source.  
  - Reinterpretive conversions: unsafe numeric conversions where the converted value can be different than the source but no data is lost.  
    - Signed int is converted to an unsigned int.   
    - Negative number gets modulo wrapped to a large integral value outside the range of an unsigned int.   
    - Why don’t they lose data? Values can be converted back into the source type even if the initial conversion produced a value  out of range of the source type.   
  - Lossy conversion: unsafe numeric conversions where data may be lost during conversion.   
    - Double to int: results in data loss  
    - Double to float: results in data loss  
    - Converting a value with lost data back to the source type results in a value different han the original value.  
  - Unsafe conversions can be used when we can constrain the values to be converted to just those that can be converted to equal values.  
    - Integer can be safely converted to an unsigned int when we can guarantee int is non-negative.   
    - We don’t bind if some data is lost (liek when convert an int to bool)  
  - General principles:  
    - Converting a value into type whose range doesn’t support that value, leads to unexpected results.  
    - Overflow: well defined for unsigned values, undefined for signedf values.  
    - Converting from integer to floating point: works as long as the value fits within the range of the floating point type.  
    - Convertin from floating point to integer works as long as the value fits within the range of the integer, but we lose any fractional values.

10.4:

- Narrowing conversion: potentially unsafe conversion where there is a chance the destination can’t hold all the values of the source type.   
  - Bunch of special cases.  
- Implicit narrowing conversions result in compiler warnings (other than signed/unsigned conversions)  
- Should be avoided as much as possible.  
  - Narrowing conversions are not always avoidable.  
- Narrowing conversions are not always avoidable (when there’s mismatch between parameter an argument ).  
  - Use static\_cast, as it suppresses compiler warnings.   
- Remember that brace initialization disallows narrowing conversions.  
- Workaround through initialization via static cast.  
- When we don’t know the soure value of a narrowing conversion, the result can’t be determined until runtime.   
- However, when the source value of a narrowing conversion is constexpr, the specific value that’s getting converted is known to the compiler.   
  - So we can just do the conversion, and then check to see whether the value was preserved.   
- Narrowing conversion: form an integral type to another integral type that can’t represent all values of the original type, unless the value being converted is a constexpr whose value can be stored EXACTLY in the destination type.  
  - So if we have a constexpr of 5, we can use that to initialize an unsigned int because 5 is exactly within the range. We can do this validation when we have constexpr at compile time.   
- Conversions from a floating point type to an integral type don’t have a constexpr exclusion clause so they are always considered narrowing conversions.   
  - Int n {5.0} → narrowing conversion.  
- Conversions from constexpr floating point type to a narrower one: not considered narrowing.   
  - constexpr double d {0.1}  
  - Float f {d}. 


10.5:

- What happens when we do arithmetic operations and the operators are of different types.  
  - Like one’s an int and the other is a double.  
- There is an implicit conversion to matching types using a set of rules called user arithmetic conversion (common type of the operands).  
- Rules:  
  - First:  
    - If the operand is an integral type and the other is a floating point type: integral operand is converted to the type of the floating point operand.  
    - Otherwise: integral operands are numerically promoted.  
  - Second:  
    - After doing a promotion, if one operand is signed and the other is unsigned, there are special rules:  
      - The rank of the unsigned operand \>= signed, the signed operand is converted to unsigned operand.  
      - If the type of the signed operand can represent all values of the type of the unsigned operand, the unsigned operand is converted to the signed operand.   
      - Else: if both are the same type, we just convert both to unsigned typed.

    - Else, the operand with lower rank is converted into the type of operand with higher rank.  
- Priority list:  
  - Long double (highest rank)  
  - Double  
  - Float  
  - Long long  
  - Long   
  - Int  
- Adding two shorts: not on the priority list, so we promote to int.

10.6:

- Literal suffixes can’t be used with variables, how do we know how to convert to floating point.  
  - We use a cast.  
- Five different types of cast:  
  - Static\_cast: compile-time type conversions between related types  
  - Dynamic\_cast: runtime type conversions on pointers or references polymorphically  
  - Const\_cast: we add or remove const   
  - Reinterpret\_cast: do a bit-level reinterpretation.  
- Each cast: takes an expression and a target type.  
- Use const\_cast and reinterpret cast in very rare situations.  
- In C, casting is done through operator ().  
  - Like (double).   
  - Can also do double(x)  
- Not good to use because we have no idea what cast is actually being done.  
- Static\_cast:m most frequently used cast, when we want to explicitly convert a value of one type into another.  
- Returns a temporary object of the requested type.  
- Static\_cast: performs compile-time type checking.  
  - Also less powerful than a normal c-style cast.  
  - Used to make narrowing conversions explicit.  
- We can cast or initialize a temporary object.  
  - What that means is if we want to convert x to an integer, we can do static\_cast\<int\>(x) or we can do int {x}.  
  - Avoid doing int(x) because that’s a C-style cast.  
- Difference between static\_cast and direct list initlization:  
  - Int{x} disallows narrowing conversions.

10.7:

- Using creates an alias for an existing data type.  
- Naming type aliases:  
  - \_t suffix (inherited from C but has falled out of favor in modern C++)  
  - \_type suffix some libraries use.  
  - Type aliases without suffix: just use capitalization (this is the norm).  
- Type alias: same scoping rules as variable identifiers (block scope, all that).   
- Can use typedefs as well to create an alias for a type,.   
  - Confusing typdef type name.  
- Type aliases allow for platform independent coding (because you can just substitute something in).  
- Allows for more interpretable and easier to debug code.

10.8:

- There’s redundancy in the variable definition double d {5.0}, since we know for sure what d is equal to.  
  - The literal passed in has to be 5.0  
- You can use type deduction with auto keyword.    
- Type deduction needs something to deduce from (can’t use void).   
- Type deduction: drops const from deduced types.   
- Type deduction for string literals defaults to c type const char\*, not std::string.  
- If you want the type to be deudced from a string literal to std::string or std::string\_view, use s or sv literal suffixes gathered through namespace.  
- Constexpr is dropped.  
- Type deduction: allows for avoiding unintentionally uninitialized variables.  
  - However, it obscures an object’s type information in the code.

10.9:

- You can use auto to do type deduction for a function as well.  
  - Can’t return two different types of values.  
- Can’t use a normal forward declaration because a forward declaration requires knowing the return type.  
  - Trailing return type allows us to do a forward declaration.   
- Type deduction can’t be used for function parameters. 