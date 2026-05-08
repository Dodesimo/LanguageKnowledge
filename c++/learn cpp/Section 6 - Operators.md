	## 6.1

- Operands: process involving zero or more input values producing an output. 
- How does the compiler evaluate compound expressions?
- Compiler parses expression and determines how operands are grouped with operators (precedence and associativity rules). 
- Operands are evaluated and operations are executed to produce a result. 
- Precedence rules: determined by operator. 
- If everything is the same precedence go from left to right. 
- `<<` left shift and insertion, `>>` right shift and extraction
- Value computation: execution of operations to produce a value.
- Evaluation: evaluation of operands. 
- Operands for a function: evaluated in any order. 
- Lef to right, all parameters are populated in left to right order. 
- Right ot elft, other way. 
- Can result in conflicting calculations.
- Clang: evaluates left to right.
- GCC: right to left.
- Program can be made unambiguous: make each function call to getValue a separate statement. 

## 6.2

- Arithmetic operators:
- Unary: `+`, `-`.
- Binary: `+`, `-`, `*`, `/`, `%`. 
- Division: has two modes. 
- If either or both of the operands are floating point values: division operator performs floating point division (will return a floating point value)
- If both operands are integers: division operator performs integer division, drops any fractional components and returns an integer value. 
- Arithmetic assignment operators;
- `+=`, `-=`, `*=`, `/=`, `%=`
- Assignment operators (normal, arithmetic, and bitwise), as well as increment and decrement operators are designated as modifying operators. 

## 6.3

- Modulo operator: returns the remainder after doing integer division. 
- If first number is smaller than the second, then its always the first number that is the modulo. 
- Remainder operator can also work with negative operands. Returns the sign of the first item. 
- Not really the actual modulo, a “remainder’ function. Look more into this behavior when doing leetcode. 
- Compare the remainder operation against 0.
- For exponents: use the cmath library and the std::pow function. 
- Parameters are of type double. 

## 6.4

- Increment and decrementing: prefix and postfix 
- Prefix: incremented and then assigned.
- Postfix: assigned and then incremented. 
- Postfix has more steps than the prefix one. 
- Prefix version: more performant. 
- Prefix and postfix: can cause problems with the way function arguments are evaluated. 
- Don’t use variable with side effect applied multiple times in a statement. 

## 6.5

- Comma operator: evaluate multiple expressions whenever a single expression is allowed. 
- Evaluate the left operand, right operand, and then return the right operand’s value. 
- Just fade this and use this in a for loop. 

## 6.6

- Ternary: `c ? a : b`, if c , then a else b. 
- Conditional operator: can be used anywhere an expression is accepted, when operands are constant expressions, conditional operator can be used 
- Conditional operator: parenthesized entire conditional operation when using a compound expression. 
- Also consider parenthesizing the condition if it contains any operators.
- Type of the second and third operand needs to match.
- Compiler must be able to convert both of the second and third operands to matching types. 
- What is the conditional operator good for?
- Initinalizing an object with one of two values.
- Assigning one of two values to object.
- Passing one of two values to a function
- Returning one of two values from a function
- Pritning one of two valuies. 

## 6.7

- Relational operators: common sense.
- Boolean conditional values: just have the boolean value.
- There can be inconsistencies in the way floating point numbers are represented in C. Less than or greater than or less tahn or equal to or greate rthan or equal to can produce reliant answers in most cases, but if the values are very similar, chance they don’t. 
- You can use less than or greater than with some care, avoid using `==` or `!=`. 
- Safe to compare a floating point literal with a variable of the same type initialized with a literal of the same type. 
- Unsafe to compare floating point literals of different types. 
- Not safe to compare floating point literals of different types. 
- Epsilon use to validate “close enough ness”
- Donald Knuth: see whether difference between two numbers is within the max of them times some epsilon multiplier. 
- Can mix this with an absolute delta approach to validate closeness. 

## 6.8

- Logical operators: 
- Not (`!`), and (`&&`), or (`||`). 
- Short circuit evaluation: if the left operand evaluates false, we return false regardless in and and, if the left operand evaluates true, we return true regardless. 
- Optimization
- And has a greater precedent than or. 
- Use parentheses.
- C has no inbuilt XOR, so we can use `!=`. 
- Can also juse us and or not (the words) instead of the symbols but the words are better supported. 