## 1.1

- Preprocessor directive: used to get library contents. 
- Return 0 it works, else no. 
- Compilers sometime report an error has occurred on the line before, sometimes there. 

## 1.2

- Comments: `//` for single line, `/*`, `*/` for multi line. 

## 1.3

- Values in single-quotes: character values.
- Values in double quotes: text values
- Numeric values: not quoted. 
- Values that are directly in source code; literals. 
- Read-only.
- Object: region of storage that can hold a value, and have associated properties. 
- With a name is called a variable. 
- Variable definition: tells the compiler we want a variable in the program.
- Runtime: each object is 

## 1.4

- Copy assignment: equals sign and then get the right hand side to the left. 
- Initialization: different forms:
- Copy initialization: directly after equals sign
- Values are implicitly copied. 
- Direct initialization: value in parenthesis
- Values are explicitly cast to another type through casting. 
- Direct list, initial value in braces
- Normal way to initialize. 
- Prevents narrowing conversions. 
- Value initialization: empty braces
- `[[maybe_unused]]` attribute: allows us to tell compiler that we are okay with a variable being unused. 

## 1.5

- Include `<iostream>`: required at the top of any code file.
- `<<`: insertion operator allows for concatenation of outputs.
- `std:endl`: add an ending line
- `Std:cout` is buffered:
- Programs request output to be sent in console, not immediately, send in a line, and then at once, the buffer is flushed and displayed to the console. 
- `Std;endl` is inefficient because it outputs a new line and flushes the buffer. 
- `>>` extraction operator
- Instead: do `"\n"`. 
- `Std::cin` is also buffered:
- First in first out buffer to ensure data is processed in the same way as it was added.
- Individual characters added as input are added at the end of an input buffer.
- Extraction operator: removes characters from the front of the buffer and converts them into a copy-assignment value. 
- Weird behavior where `std:cin` will read from the buffer already if there’s data there.
- Leading whitespace characters are disregarded from the input buffer and the next character is used. 

## 1.6

- Uninitialized variable: gets garbage information. 
- Initialized: object is given a known value at the point of definition. 
- Assignment: object is given a knowledge value beyond the point of definition. 
- Uninitialized: object hasn’t been given a known value
- Using the value of an uninitialized variable is the first example of undefined behavior. 
- C++ doesn’t have rules for this.
- Implementation-defined behaviors: enables determination of how some aspect of language will behave. 
- Unspecified behavior: similar to implementation-defined behavior in that behavior is left up to the implementation, but not required to document the behavior. 

## 1.7

- C++ identifier: name of the variable, function, type: called an identifier. 
- Identifier rules:
- Cannot be a keyword.
- Only composed of letters, numbers, or the underscore character.
- ID must start with letter.
- Case sensitive. 

## 1.8

- Whitespace:
- Preprocessor directives must be placed on separate lines. 
- Common space whitespace stuff

## 1.9

- Literal: fixed value directly added to the code.
- Can not be changed it is fixed in value.
- Values that are inserted directly into source code. 
- Objects and variables: memory locations holding values, can be fetched on demand. 
- Operators: performs an operation across a series of operands.
- Addition, subtraction, multiplication, division, assignment, insertion, extraction, equality. 
- Number of operators that are keywords: new, delete, throw.
- Number of operands an operator takes as input: arity.
- Unary: one operand. 
- Binary: operates on a left and right operand.
- Ternary: operates on three, conditional operator
- Nullary operator: acts on zero operands, throw operator
- Side effect: operator that has an observable effect beyond producing a return value is a side effect. 
- We call certain operators just for their side effects, they return the left operand

## 1.10

- Expression: non-empty sequence of literals, variables, operators, and function calls that calculate a value.
- Process of executing an expression: called evaluation, resulting value is called a result. 
- Literals evaluate to their own values. 
- Variables evaluate to the value of the variable. 
- Expressions don’t end in a semicolon and can thus not be compiled by themselves. 
- Expression statement: expression plus a `;`. 
- Subexpression: expression used as an operand, in `x = 4 + 5`, `x` and `4 + 5`
- Full expression: expression that is not a subexpression, `2`, `2 + 3`, `x = 4 + 5`. 
- Compound expression: expression with two or more operators, `x = 4 + 5`, 