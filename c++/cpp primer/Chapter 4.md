- Unary operators: act on one operand.
- Binary operands; act on two operands.
- One ternary operator takes three operands.
- One operator, function call, takes an unlimited # of operands.
- Operands can be converted.
- Overloaded operator: give a different meaning when applied to class types.
	- Number of operands, precedence, and associativity of the operator cannot change.
- lvalues: have identifiable memory locations (can be associated with an exact memory location).
	- use an obj. as an l-value, use objs. identity (address)
- rvalues: temporaries/literals that don't have identifiable memory locations.
	- use an obj. as a r-value, use its value (contents)
- Can use an l-value where an r-value is required, but can't use an rvalue when an l-value is required.
- `decltype`: yields lvalue if expression yields an lvalue.
	- since address-of returns rvalue, `decltype(&p)`, where p is `int *`, returns `int**`.
- Precedence and associativity laws determine operand grouping in a compound expression (2+ operators)
	- Override these rules through parenthesization.
	- Higher precedence: group more tightly than operands w/ lower precedence.
	- Associativity: determines how to group operands with same precedence.
- Arithmetic operators: left associative, operators at same precedence level group left to right.
	- This means if we have multiplication and then division, multiplication done first then division.
- Precedence specifies how operands are grouped.
	- Doesn't say in which order operands evaluated.
- For example in `int i = f1() * f2()`, no way to know that `f1()` gets called before `f2()`.
- Operators that don't specify evaluation order, error to refer and change the same object.
	- Undefined behavior in situations like:
		- ```c++
		  int i = 0;
		  std::cout << i << "" << ++i << std::end;
		  ```
* Four operators guarantee order in which operands get evaluated:
	* AND (&&) --> left gets eval first.
	* Right eval. only if left eval.
	* OR (||), conditional operator, and comma.
* Order of operand evaluation is independent of precedence and associativity:
	* `f() + g() * h() + j()`:
		* Precedence: tells us g and h get multiplied.
		* Associativity tells us `f()` gets added to the multiplication, and then that gets added to `j()`.
		* No guarantees to the order in which functions are called.
	* If these are all independent functions, doesn't matter.
	* If any of them affect same object, then expression is undefined.
* Arithmetic Operators:
	* Unary has higher precedence than multiplication/division.
	* Higher than addition/subtraction.
	* Can be applied to all arithmetic types or things converted to arithmetic types
	* Operands + results: rvalues
	* Small types are promoted.
	* Unary +: when applied to pointer or arithmetic value, returns a possibly promoted copy of its operand value.
	* Unary -: returns result of negating possibly promoted copy.
	* Interesting case:
		* `bool b = true, bool b2 = -b`: b2 is true since true is 1, negative of that is -1, and then that evaluates to true since false i 0.
	* Signed overflow: is a undefined behavior.
	* Division: truncates towards 0.
	* Modulo: operands must be integral type.
	* Quotient: rounded towards 0.
	* Division rule: `a = (a / b)*b + (a % b)`
* Logical/Relational Operators:
	* Precedence !, < + <=. + > + >=, == + !=, &&, ||
	* Take operands that can all be converted to `bool`.
	* All return boolean types.
	* Arithmetic and pointer operands with a value of 0 are false.
		* All others are true.
	* All results take in rvalues and return rvalues.
	* Logical AND: true if both true. 
	* Logical OR: true if one true.
	* Both evaluate left before right.
		* Right is evaluated only if left doesn't determine result (short-circuit evaluation).
		* Use the left-hand condition to test if the right hand condition is safe to evaluate.
	* Reminder that references avoid copies.
	* Logical not: straightforward.
	* Relational operators: 
		* Left associative, return bool values.
	* To test truth value of something, we can do `if (val)`.
		* Don't do `if (val == true)` b/c long and when val isn't a bool, true gets converted to the type of the val before `==`
* Assignment Operators:
	* Left-hand operand: modifiable lvalue.
		* Result of assignment: type of the left-hand operand.
		* Types of left and right operands differ: right-hand operand converted to the type of the left.
		* Value initialization: can't be narrowing.
		* If initializer list is empty, compiler generates a value init. temporary and assigns that to the left-hand operand.
	* Assignment: right associative.
		* ```c++
		  int ival, jval;
		  ival = jval = 0; //right most assignment jval = 0 is the right-hand operand of left-most assignment operator, assigned to ival.
		  ```
	* Each object has the same type of right-hand neighbor or a type that neighbor can be converted to.
	* Assignments have low precedence (occur in conditions).
		* So use parenthesis around assignments in conditions.
	* Confusing equality and assignment operators:
		* `if (i = j)`:
			* This assigns j to i, and then tests i.
	* Compound assignment:
		* Left-hand operand is evaluated once.
		* Versus writing out to `a = a OP b`, the left hand operand gets evaluated twice.
* Prefix/Postfix operators:
	* Prefix: increments/decrements operand, yields changed obj. as aresult.
	* Postfix: increments/decrements operand, but yields copy of original value.
	* Both operators require lvalue operands.
	* Prefix: return object as an lvalue
	* Postfix: return copy of object's original value as a rvalue.
	* Postfix increment precedence is greater than that of dereference.
	* Evaluation in any order creates ambiguity:
		* ```c++
		  while (beg != s.end() && !isspace(*beg)) {
			  *beg = toupper(*beg++); //undefined because we could either evaluate
			  *(beg + 1) = toupper(*beg) // or
			  *beg = toupper(*beg)
		  }
		  ```
			- Don't want situations where a subexpression changes value of operand used in another.
	-  Member access operators:
		- dot operator: fetches member from an object of class type.
		- Arrow: access member through a field.
	- Dereference has a lower precedence than dot, parenthesize.
	- Arrow operator: requires a pointer operand, yield an lvalue.
	- Dot operator: yields lvalue if object is an lvalue, else rvalue.
- Conditional Operator:
	- `condition ? expression1 : expression2`
		- expression1 and expression2: same type or can be converted to a common type.
	- If condition true, we evaluate expression1 else expression2.
		- Result is an l value if both are lvalues or they convert to a common l value type or the result is an rvalue.
	- Conditional operator: right associative.
		- Operands group right to left.
		- Accounts for the fact that right-hand forms the : branch of the left-hand.
- Bitwise operator:
	- Operands of integral types as collection of bits.
	- Left associative: 
		- Order: NOT, left shift + right shift, and, xor, or.
	- If signed operand, and value is negative, way that the sign bit is handled is machine dependent.
		- Left-shift changing value of sign bit is undefined.
	- `<<` and `>>`: right-hand operand must be nonnegative and less than # of bits in result.
		- Bits that are shifted off the end are discarded.
	- Left-shift: inserts 0 valued bits on the right.
	- Right-shift: depends on type of left-hand: if operand is unsigned, insert 0. Else 1 (extend sign) or 0-valued bits depending on implementation.
	- Bitwise not:
		- Char (other small integers) operand gets promoted to int.
		- All bits are negated in their value.
	- Shift operators: midlevel precedence (lower than arithmetic operators, higher than relational, assignment, and conditional operators).
- `sizeof` operator:
	- returns size in bytes of an expression, right associative.
	- Constant expression of type `size_t`, takes one of two forms:
		- sizeof(type), sizeof expr
	- Expression is not actually evaluated (so can pass in an uninitialized pointer).
	- Dereferencing invalid pointer as the operand is safe.
	- Can use the scope operator for the size of a member of a class type (can only access members through an object, but we don't need an object for `sizeof`).
	- `sizeof(char)` is guaranteed to be 1
	- `sizeof(referenced type)`: size of the object of the referenced type
	- `sizeof(pointer)`: size needed to hold a pointer
	- `sizeof(dereferenced pointer)`: size of object that pointer points to.
	- `sizeof(array)`: size of the entire array (doesn't convert to pointer)
	- `sizeof(string or vector)`: returns size of fixed part not the size  used by the object's elements.
	- number of elements in an array: divide the array size by the element size.
		- sizeof(array)/sizeof(*array)
	- can use sizeof to specify the dimension of an array.
- Comma operator:
	- Like AND, OR, and CONDITIONAL, comma guarantees order in which operands are evaluated.
		- left-hand expression evaluated, result is discarded as right hand expression is returned.
		- Result of a comma expression is value of right-hand expression.
		- Result is an Lvalue if the right-hand operand is a lvalue.
		- Used in while loops to test something first and then use that as a side-effect for the second expression.
* Type Conversion:
	* Two types are related if there's a conversion between them.
	* Implicit conversion: done automatically.
	* Defined to preserve precision.
		* Expression has both integral and floating point operands: integer is converted to floating point.
	* Instances of implicit conversions:
		- Values of integral types smaller than int are promoted.
		- Conditions: nonbool expressions are converted to bool.
		- Initializations/assignments: initializer is converted to variable type, assignments, right-hand operand is converted to type of left-hand.
		- Arithmetic and relational expressions: types are converted to common type.
		- Conversions happen in function calls.
	- Arithmetic Conversions:
		- Convert to widest type.
		- Integral values are converted to floating point type.
	- Integral promotions:
		- `bool, char, signed char, unsigned char, short, unsigned int`: all promoted to int.
			- If all possible values of that type fit in an int.
		- Else: unsigned int.
		- Larger character types: promoted to smallest type of int where all values can fit.
		- Operands are unsigned:
			- If operands have different types, converted to common.
			- Any operand is an unsigned type, type of operands depend on relative size.
			- Integral promotions happen first.
			- Both promoted operands have same signed-ness:  operand w/ smaller type converted to larger type.
			- Signed-ness differs and type of unsigned operand is same or larger. than signed, signed goes to unsigned.
			- Signed is greater than unsigned: result is machine dependent.
				- Unsigned fit in signed: unsigned is signed.
				- Unsigned doesn't fit in signed: signed is unsigned.
	- Array to pointer: implicit conversion.
		- Not performed when array is used w/ decltype, sizeof, typeid, or reference.
	- Pointer can be converted:
		- Constant integral value of 0.
		- literal nullptr
			- Both can be converted to any pointer type.
		- Pointer to any nonconst type: converted to void*.
		- Pointer to any type can be converted to const void*.
	- Conversion to bool:
		- Automatic conversion or pointer types to bool.
		- Pointer or arithmetic value is zero, conversion is false.
			- Else true.
	- Conversion to const:
		- Convert a pointer to nonconst type to a pointer to the corresponding const type.
		- Similar for references.
		- Can't do reverse.
	- Conversion by class-types: compile only does one class-type at a time.
		- Example: using a C-style character string when a library string is expected.
		- `cin`:: if last read succeeded, conversion is true, else false.
- Explicit Conversions:
	- Explicitly convert objects --> use a cast.
	- Named cast: `cast-name<type>(expression)`:
		- if type is a reference: result is an lvalue.
	- `static_cast`:
		- Any well-defined conversion other than low-level const.
		- Useful when larger type is assigned smaller. 
		- Also useful for conversions a compiler won't generate automatically.
			- Implicitly converted pointer to void (if nonconst).
			- Getting the pointer back: requires a cast.
			- Needs to be the actual OG type, otherwise undefined result.
	- `const_cast`:
		- Changes low-level const in its operand.
		- Converts a const object to a nonconst object.
		- Compiler doesn't prevent from writing to it.
		- If object wasn't originally a const, this is legal.
		- But if the og object was const, this is undefined.
			- This means that if we have a const pointer to a non-const object, we can use const-cast to enable edits.
		- Can only use a const cast to change constness of an expression.
			- Anything else compile-time.
		- Can't use `const_cast` to change type of expression.
	- `reinterpret_cast`:
		- does low-level reinterpretation of bit pattern of operands.
		- allows arbitrary reinterpretation of the data type:
			- ```c++
			  int *ip;
			  char *pc = reinterpret_cast<char*>(ip);
			  ```
		* Can't Just pass this in as a normal character pointer.
			* Keep in mind the original data type.
		* Compiler doesn't know whether the pointer is actually to an int.
			* Meaningless/useless interpretation that can cause weird runtime behaviors.
Refer to the  precedence table.