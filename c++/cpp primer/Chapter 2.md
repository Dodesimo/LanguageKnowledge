* Const must be initialized.
* Copying with `const` does not affect const-ness of what gets copied into.
* Const variables are defined local to the file.
	* If initialized with a compile-time constant, it's like we wrote definitions for separate variables in each file.
	* Declare a single instance, use `extern` keyword on both definition and declaration.
* Reference to const: reference cannot be used to modify the underlying object.
	* Given a const object, can't have a reference to non-const because that reference could modify existing object.
* Temporary object: compiler-created to store result from expression evaluation.
	* Binding a reference to a temporary is illegal due to const-ness.
* Reference to const does not say anything about underlying object being const or not.
* Pointer to const: pointer cannot be used to change the underlying object.
* Const pointer: pointer itself cannot have its address be changed, and once init., value can't be changed.
* Top-level const: pointer itself is a const (only one in non-compound type, right-most) 
* Low-level const: object itself is const (left most).
	* Const in reference types is always low-level.
* Copying an object: top-level consts are ignored.
	* Since copying an obj. doesn't change the copied object.
* Copying an object: low-level consts aren't ignored.
* Constant expression: value cannot change and can be evaluated at compile time. 
	* Literal + const object initialized from const expression.
* `constexpr`: implicitly `const`, initialized with constant expressions.
	* Can only be used with literal types, like arithmetic, reference, and pointer types.
	* For pointers: can only be `nullptr` or a literal 0, or obj. in a fixed address.
	* For references: obj. in a fixed address (outside a function).
	* Imposes top-level const.
* type alias: synonym for another type.
	* `typedef OG ALIAS`: base of a declaration.
	* `using ALIAS = OG`
* Compound alias: `const` casts entire type (can't just textually sub it in.)
	* Base type of an alias would be pointer. 
* `auto`: tell the compiler to deduce the type from initializer.
	* So must be initialized.
	* Reference as iniitalizer: reference object's type used for type deduction.
	* Top-level consts are ignored. 
	* Can bind a const reference to a literal.
	* Reference to auto-deduced type, top-level consts are not ignored.
* `decltype`: define a variable compiler deduces from expression, but doesn't use the expression to initialize a variable.
	* just analyzes expression, determines type.
	* returns type of variable, including top-level const and references.
	* applied to a variable defined as a reference, it is not a synonym for the object.
	* returns reference type for expressions that can stand on the left side of an assignment (so dereferencing)
	* wrapping in parenthesis: causes evaluation on the left-hand side of an assignment. 
		* `decltype((i))` -> results in a reference of i.
		* `decltype(i)` -> results in the actual type of i
* Preprocessor:
	* runs before the compiler and then changes source text
	* use it to protect against multiple inclusion.
	* `$ifndef`, `#define`, `#endif`
		* This ensures that only one header is added.
		* 