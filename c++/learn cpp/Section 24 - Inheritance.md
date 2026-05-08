24.1:

- . inheritance: new objects acquire attributes and behaviors of others and extend them. 

24.2:

- Parent class, base class, superclass → child class, derived class, subclass.  
- To inherit through public inheritance: do : public Person.  
- All public fields are inherited (this includes variables and functions)  
- You can chain inheritance (we know this).   
  - Prevents redefinition as we automatically receive member functions and member variables of the base class.

24.3:

- What happens during initialization of a base class versus a derived one?  
- Base:  
  - Allocate memory for the class and then call the default constructor to do initialization.  
- Derived:  
  - Most-base class is constructed, each child is then constructed until the most-child class at the bottom is constructed last.  
  - Makes sense because a child uses variables and functions from the parent, but the parent knows nothing about the child.

24.4:

- What happens when a base class is instantiated?  
  - We set memory aside.  
  - Call the construc  
  - tor.  
  - Initialize based on the member initializer list.   
  - Body of the constructor executes.  
  - Control is returned to the caller.  
- What happens when a derived class is instantiated?  
  - Memory for derived is set aside (Base and Derived portions)  
  - Appropriate derived constructor called.  
  - Base object is constructed with the appropriate base constructor.   
  - Member initializer list initializes variables  
  - Body of the constructor executes.  
- Classes cannot initialize inherited member variables in the member initializer list.  
  - This is to prevent derived class constructors from changing values.  
- For a derived class, to specify base class constructor, add the constructor in the member initializer list.  
- When we have base and derived:  
  - Memory for the derived object is allocated.  
  - Derived constructor is called.  
  - We see if there’s a particular base constructor, thats called.  
  - Base class constructor member iniitalizer list is called.  
  - Body executes.  
  - Returns  
  - Derided class constructor member iniitalizer list executes.  
  - Body executes.  
  - constructor returns.  
- Private members of the base class cannot be directly accessed.  
- You can only call constructors from their immediate parent/base.  
- When a derived class is destroyed, destructors get called in reverse order.

24.5:

- Public members can be accessed by anybody.  
- Private members can only be accessed by member functions of the same class or friends.  
- Protected specifier: allows class the member belongs to, friends, and derived classes to access member.  
- Private members: can only be accessed by Base members and friends.  
- Benefits of protected: easy changes, but if we change anything later, base and derived classes need to be changed.  
- Private: enable invariants to be maintained, but class needs to have a larger public interface to support all of the functions.  
- C++ defaults to private inheritance.  
- When members get inherited, access may be changed depending on the type of inheritance.  
- Public inheritance:  
  - When a base class gets inherited publicly, public members stay public, protected members stay protected, private members stay inaccessible. (doesnt really change anything)  
- Protected inheritance:  
  - Public and protected members become protected, and private members stay accessible.  
  - Very rare.  
- Private inheritance.  
  - Members from base class are inherited as private.  
  - Private members are inaccessible, protected and public members become private.  
- Derived classes:  
  - Can access all public and protected members in top class, but cannot access private members.  
  - When interacting with derived class objects, the form of inheritance alters what fields can be accessed.  
- Only have access to direct parent class.

24.6:

- Base classes don’t have access to derived methods.

24.7:

- When a member function gets called in a derived class object, the compiler sees if there’s any function in the derived class.  
  - If so, all overloaded functions are considered, then resolution occurs to pick the best match.  
  - If not, go up the inheritance chain.  
- Compiler picks the best matching function from the most derived class with at least a function with that name.  
- Access specifiers can be redefined in the derived class for functions.  
- Gain access to the original function call through scope resolution operator ::.  
- To gain access to a friend method of the base class, static cast.  
- If the compiler finds a function in the Derived class, it will only consider those functions when determining what function to resolve to.   
- Using Base::function declaration allows for base functions to be used in resolution.

24.8:

- Hiding inherited functionality.  
- We can change a member’s access specifier in a derived class, using declaration.  
- Can only change access specifiers of base members derived class would be normally able to access.  
  - So you can’t change the access specieifer of a base member to protected or public.  
- We can change access specifiers to encapsulate data.  
- Cannot only change accessibility for a single function, using command changes the access for all overloads.

24.9:

- Multiple inheritance: a derived class inherits members from multiple parents.  
- Just specify each base class separated through commas.  
- Mixin class: inherited from in order to add properties to a class.  
  - Best practice to specify through scope resolution.  
- Issues:  
  - Ambiguity exists when multiple base classes have a function with the same name.  
    - Multiple parent classes may have the same function.  
  - Diamond of doom where two parent classes end up inheriting from the same main class.
