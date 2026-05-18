- Classes: data abstraction and encapsulation
- Interface: operations users of the class can do.
- Implementation: data member sof the class, bodies of the functions, functions needed to define the class.
	- A class separates these two concepts. 
- Encapsulated class: hides implementation, users can use the interface, but can't access the implementation.
- abstract data type: defined by its behavior, not its implementation.
- Member functions: must be declared inside the class.
	- May be defined inside the class itself or outside the class body. 
- Non-member functions part of the interface declared and defined outside the class body.
- Functions defined in the class: implicitly inline. 
- Call a member function, do it on behalf of an obj.
- Member functions access the object on which they were called through an implicit param called `this`.
	- Calling a member function: sets `this` to address of object invoked upon.
	- Inside class: can directly use name of member variables and its bound to whatever this param refers to. 
	- `this`: implicitly defined `const` pointer, cannot change address it points to. 
- `const`after function parameters.
	- `this` is a const pointer to a non-const version of the object.
	- so can't use `this` on const-version of the object.
	- so `const` following param list: `this` is a pointer to const.
		- `const` member functions
- Class compilation:
	- Member declarations are compiled first.
	- Member function bodies are processed. 
	- So can use function bodies regardless of where they appear. 
- Defining a function outside a class:
	- Specify class names with scope operator and then name. 
	- Return type, param list, and name need to match.
- to return the object we operate on, can just dereference `this`  pointer. 
- functions conceptually part of a class, but not defined inside class, typically declared inside the same header.
- constructors: how classes control object initialization by defining one or more member functions. 
	- no return type
	- name as the class
	- cannot be const: const objects can be written to by the constructor during their construction. 
	- default constructor: 
		- if there's an in-class initializer use that.
		- else default init.
		- only generated if no constructor. 
	- This poses a problem if there's arrays or pointers inside.
	- Some classes: define their own constructor when compiler can't synthesize a default one ()
- Given a class `Sales_data`:
	- default constructor `Sales_data() = default`;
		- Can be done inside the class body: in-lined
		- if is done outside the class, not inlined.
	- constructor initializer list:
		- `Sales_data(const std::string &s): bookNo(s)` 
		- specifies initial values for one or more data members of the object getting created. 
		- list of member names, each of which followed by member's init value in parenthesis/curly braces.
		- if init values are missing, they use  in-class initializers or are default initalized. 
- Copy assignment and destructor:
	- Objects are copied: such as when we init. a variable or pass/return obj by value.
	- Objects are assigned when they use an assignment operator.
	- Objects are destroyed when end of scope. 
	- Compiler provides defaults.
	- Don't work correctly for classes allocating resources outside the class objects themselves. 
		- If a class has `vector` or `string` members, object destroyed, the compiler provided destructor takes care of it as vector class has ways to destroy all elements. 
- Use access specifiers for encapsulation. 
	- `public` members: available to to all parts of the program
	- `private` members: accessible to member functions of the class, not accessible to code that uses class. 
- `struct`: members are by default private
- `class`: members are by default public`
- A class allows another class/function to access its internals by making it a friend. 
	- Not members of the class
	- appear anywhere in the class
	- not affected by control access
- benefits of encapsulation:
	- user code can't corrupt state of encapsulated object
	- implementation in encapsulated obj. can change over time without needing user changes. 
- need friend declaration in class and friend declaration after class.
- type declaration: if public, forces callers to use that type for params. 
	- defined types: appear at the beginning of the class. 
- can specify inline on the declaration inside class or the definition outside.
	- inline member functions defined in the same header.
- `mutable`: class has a data member we want to modify even within a const member function.
	- `mutable`: never const (so even a const member function can change it).
- in class initialization: either through `=` or braces. 
- if definition is outside class, to be `inline`, both declaration and definition need to have `inline` keyword. 
- be sure returning values of a current object is a reference (otherwise, a copy would be returned).
- const member function that returns `*this` should have return type reference to const.
- Have const overloads:
	- Two versions of function, one is normal reference returning the other is a const function only returning a const reference:
		- `Screen &display (std::ostream &os)`
		- `const Screen &display const (std::ostream &os)`
- Two classes w/ the exact same member list have different types. 
- can declare a class without defining it.
	- forward declaration: introduces the class to the program 
	- before definition: type is incomplete.
- Data members: specified to be of class type only if class has been defined. 
- Data members can be of a class type only if they have been defined.
	- Can't have instances of itself, but since we have declared but not fully defined, can have pointers.
- Friends:
	- Can make another class a friend.
	- Can declare specific member functions of another class as friends.
	- Friend function can be defined inside class body. 
	- friend class:
		- member functions can access all members, including nonpublic ones. 
	- can make indiv. functions friends 
- Each overload needs to be individually made a friend.
- Access type names from class using scope operator.
	- Ordinary data, function members, accessed through obj., reference, or pointer. 
- Every class defines a new scope.
	- For member definitions outside, use scope parameter and get access to member variable. 
- Member function defined outside class body: return type must specify class of which its a member.
	- This is because return types are seen before the class.
- Name lookup:
	- Look for declaration in which name was used.
	- If name not found, look in enclosing scopes.
	- If no declaration then program errors.
- Class definitions:
	- First, member declarations are compiled.
	- Function bodies are compiled only after entire class has been seen.
		- Because of this, member function bodies can use any name defined inside the class.
	- Only applies to names used in the body of a member function.
	- Names used in declarations, including names used for the return type and types in param list must be seen before used (at least declared).
		- If not found within the immediate class, look in the enclosing scope.
	- If class uses a name from outerscope and that names a type, class can't redefine the name.
	- Name in the body of a member function:
		- Look for a declaration inside the member function: only declarations in the body preceding the use.
		- If not found, look inside the class.
		- If not found, look for declaration in scope before member function definition.
		- To overrwrite the parameter (use `this->` to gain access to )
		- Can access a global variable with a scope operator.
	- If a method is defined outside a class:
		- We can do resolution at the global level for functions.
		- Even though defined outside, can access members inside the class.
- Constructors:
	- When variables are defined, we typically initialize instead of define + assign.
	- If member not init. in constructor init list, default init.
		- So can we assign values to these members within the body of the constructor.
	- Can often but not always ignore the distinction between a member being initialized or assigned.
		- Const members or references need to be initialized.
	- By the time constructor body runs, init. is complete (only change to init is in the constructor init).
	- Use the constructor init list to give values for const members, reference, or class types that don't have default constructor.
	- Order of initializations not specified in constructor init. list.
		- Members init. in the order they appear in the class definition.
		- Order matters if one is used to init another.
		- Good practice: write constructor init. in the same order as members declared.
			- Avoid using members to init. other members.
	- If a constructor can be called without arguments, constructor defines a default constructor for the class.
		- So a constructor that supplies default arguments for all params. defines the default constructor.
		- `Sales_data(std::string s = ""): bookNo(s) {}`: can be called without any params, suffices default.
	- Delegating constructors: uses another constructor to perform init.
		- Delegating constructor: has a member initializer list and function body.
			- Member init list: single entry and the name of the class itself.
			- ```c++
			  Sales_data(std::string s, unsigned cnt, double price): bookNo(s), units_sold(cnt), revenue(cnt*price) {}
			  Sales_data():: Sales_data("", 0, 0) {} // init list
			  ```
			* Constructor delegates to another constructor: constructor init list and function body are both executed.
	* Role of the default constructor:
		* Used when an obj. is default or value initialized.
		* Default init:
			- define nonstatic variables or arrays at block scope without initializers.
			- class has members that use default constructor
			- members of class type are not explicitly initialized in a constructor initializer list.
		- Value init:
			- Provide fewer initializers than the size of the array.
			- Define local static object without init.
			- Explicitly request value init.
	- Most vexing parse:
		- When trying to invoke default constructor, appears to be a forward declaration for a function.
			- ```c++
			  Sales_data obj(); //this appears to be a forward declaration of a function named obj that takes no params and returns a Sales_data object.
			  if (obj.isbn() == Primer_5th_ed.isbn()); //errors
			  ```
		* instead, just leave the ().
	* Implicit class-type conversions:
		* All constructors that can be called with a single argument: defines implicit conversion to class type.
		* This allows us to pass in params that have implicit conversions.
			* Automatically create objects from given type through temporaries.
		* Compiler: will automatically apply only one class-type conversion.
	* Prevent implicit conversions through `explicit`.
		* `explicit Sales_data(std::istream&)`
		* Constructors that take in more than one parameter don't need to be designated as explicit.
			* They aren't considered for implicit conversions.
		* For explicit constructors, cannot use copy form of initialization.
		* `static_cast`: can use explicit constructors.
	* Aggregate classes:
		* Give users direct access to members and has special init. syntax.
		* A class is an aggregate if:
			* all data members are public
			* no constructors 
			* no in-class initializers.
			* no base classes or virtual functions.
		* Can initialize data members of aggregate class w/ a braced list of member initializers
			* ```c++
			  struct Data {
				  int ival;
				  std::string s;
			  }
			  Data val1 = {0, "Anna"};
			  ```
		- Must be initialized in the right order.
		- If list of init. has less elements than class has members, trailing members are value initialized.
	- Not a good idea:
		- All data members are public, user must correctly initialize every member, if new members added add inits are updated.
	- Literal classes:
		- Have function members that are constexpr.
		- Aggregate class whose data members are all literal type: literal class
		- Nonaggregate class: literal class.
			- Data members are all literal.
			- Class has at least one constexpr constructor.
			- If data member has in-class initializer, must be constant expression or constepxr function.
			- Default definition for its destructor.
		- constexpr constructor: can be default, but must have no return statement and only executable statement is return.
			- Must initialize every single data member (through either constexpr constructor or constant expression).
			- used to generate objects that are `constexpr` and for params or return types in `constexpr` functions.
	- Static class members:
		- Need members associated with class instead of individual objects.
		- use `static`: public or private, can be const, reference, array, class type and so forth.
		- static members of a class exist outside objects (don't contain static data members).
		- static member functions aren't bound to any objects (they don't have a this pointer).
		- access a static member through the scope operator.
		- Use an object, reference, pointer of class type to access a static member.
		- Member functions can use static members directly without a scope operator.
			- Can define static member functions inside or outside class body.
			- Don't repeat `static` keyword in definition (just declaration).
		- Static data members are not defined when objs. created.
			- Not init. by class constructors.
		- Cannot initialize a static member inside the class (do it outside the class body).
			- ```c++
			  class Account. {
				  private:
					  static double interestRate; //just a declaration
			  };
			  double Account::interestRate = calculate(); //define the static var outside.
			  ```
		* Continue to exist until program completes.
			*  Once class name is seen, remainder of definition is in the scope of the class.
		* Can provide in-class initializers for static members w/ const integral type + static members that are constexprs of literal types.
			* This is because they can be subbed in.
		* But if more than just subbing in, need a definition outside the class as well.
			* No need for specifying the initializer outside.
	* Since static exist independently of any other object:
		* Can have the same type as a class type which it is a member of
		* Can also be a default argument.