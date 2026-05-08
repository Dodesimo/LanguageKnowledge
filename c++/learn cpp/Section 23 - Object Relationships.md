23.1:

- Object relationships: is a, has a, those type of relationships.  
- Similar to Java object oriented programming.

23.2:

- Object composition: we build complex objects from simple ones.  
- Complex object: whole or parent, simpler object: child or component.   
- Structs and classes: built from simple parts, known as object composition.	  
  - Thus called composite types.  
- Composition:  
  - Part is part of the object.  
  - Part can only belong to one object at a time.  
  - Part’s existtnece is managed by the object.  
  - Part doesn’t know about the existence of the class (unidirectional relationship)  
  - “A part of” relationship.  
  - Object manages part’s lifetime such that user of the object isn’t involved.   
- Says nothing about the transferability of parts.   
- In C++: compositions easiest to implement, created as structs with normal data members.   
- Compositions with dynamic alloc/dealloc are done through pointer data members.   
  - If you can design a class with composition, you should.   
- Most compositions create parts when composition created, destroy when composition destroyed.  
  - Creation may be deferred till needed.  
  - Use a provided part.  
  - Delegate destruction to some other method.  
- Composition allows individual classes to be self contained, enabling reuse.   
  - Each class should do a single task. 

23.3:

- Aggregation:  
  - Part is part of the object.  
  - Can belong to multiple objects at a time (different than composition)  
  - Doesn’t have existence managed by the object (different than composition)  
  - Does not know about the existence of the object.     
- Aggregation is not responsible for part creation or destruction.   
- For example: a person and their home address.  
  - Multiple people have the same address.  
  - Address isn’t managed by the person.   
- “Has a” relationship.   
- Implemented in a class as a pointer or reference.  
  - So taken as construction parameters.  
  - Or later added through functions or operators.  
- So when the class is destroyed, pointer or reference member variable is destroyed (but not deleted).  
  - Reference is destroyed but the actual object is not.  
- Compositions vs aggregations:  
  - Compositions: use normal member variables, use pointers to handle allocation/deallocation, responsible for creation/destruction of parts.  
  - Aggregations: pointer or reference members pointing to objects outside the scope of aggregate class, not responsible for creation/destruction of parts  
- Can mix concepts of both, more dangerous (they don’t handle allocation of parts).   
- Aggregate class: class with no constructors, destructors, or overloaded assignment, all public members, just a normal struct of data.  
- We can store a series of reference through std::reference\_wrapper.ddoes

23.4:

- Association:  
  - Weaker type of relationship where there is no implied whole/part relationship.  
- What qualifies?  
  - Member is unrelated to the class.   
  - Member can belong to multiple classes.  
  - Member’s existence is not managed by the class.  
  - Member may or may not know about the object.  
- “Uses a” relationship.  
- Reflexive association: relationship with others of the same type.  
  - Course prerequisites: make it a const pointer so you can traverse through a chain of them.  
- Associations can be indirect: use an ID, doesn’t have to be a pointer.  
  - Saves space (not an expensive/spacious pointer), pointers can also reference things not in memory.

23.5:

- Dependency:  
  - Object invokes another object’s functionality to accomplish a task.  
  - Weaker relationship than an association.   
  - Always unidirectional.  
- For example: when doing an overload of \<\< to print causes a dependency on std::ostream.  
- Difference between dependency and association:  
  - Association: relationship where class keeps an indirect or direct link to an associated class as a member.  
    - Ids for example.  
  - There are no members for a dependency.  
  - Object being dependent on is instantiated as a need or passed into a function as a parameter.

23.6:

- Container classes.   
- Designed to hold and organize multiple instances of another type.  
  - So allows insertion, removable, count how many objects, sort, etc.  
- “Member of” relationship.   
- Value containers: compositions storing copies of objects and are responsible for creating and destroying those copies.  
- Reference containers; aggregations that store pointers or references to other objects.   
- Containers can only hold one type of data typically.  
- This is DSA homework for interview prep look into how each of the standard data structures are implemented. 

23.7:

- How do we make constructors with initializer lists?  
- Std::initializer list, need to specify type, return a size(), passed by value.  
- Initializer list can be passed in by iterator beginner and end.   
- Cannot access std::intializer list through subscripting.   
- Non empty initialize lists always favor a matching initializer\_list constructor over other matching constructors.   
  - This is why () gives us a size constructor (allocate of a particular size), {} list initializes.  
- Adding a list constructor to an existing one may break program.  
- Give a list constructor, also give a list assignment
