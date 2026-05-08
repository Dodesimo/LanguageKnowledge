26.1:

- You can create template classes.  
- Writing a method outside the class, use \<T\> for scoping.   
- If you don’t ever use a template class, the compiler doesn’t compile it.   
- Compiler instantiates a class template iff the class template is used in a translation unit.  
  - Solution: put all template class code in the header file.   
  - Also possible use iln.   
- Also possible to use a separate file.

26.2:

- Classes can also use non-type parameters.  
  - We pass in a constexpr value as an argument.  
- Thai sis how std::array works, the 5 is a non-type parameter.  
- If instantiating a template non-type parameter with a  non-constexpr value, doesn’t work. 

26,3:

- When instantiating a function template: compiler stencils a copy of the templated function and replaces template type parameters with actual types being used.  
- Particular functions will have the same implementation details for each instanced type.  
- Within a class, use template\<\> and then hard-code the type of the template for that particular function to be chosen.   
  - Must have the same signature at the primary template (so one can’t do pass by value the other does pass by reference).   
- Again, if matching template function function and matching non template function exist, non template function takes precedence.  
- Not implicitly inline, so inline to avoid one definition rule.   
- This doesn’t work for situations with functions.

26.4:

- Class templating may be inefficient, like if you have bool, there’s wastage of 7 bits (only one bit is used)  
- Class template specialization: we can specialize a template class for a particular data type.   
  - So we can write a customized version of storage8\<bool\> over generic storage8\<t\>  
- Completely independent classes, even though instantiated in the same way (can change the implementation).   
- For single member function changes, we don’t need to create a specialized class for the whole class, we can just do a single specialized member function. 

26.5:

- Situations where we want flexibility in one of the template types.  
- Partial template specialization: allows for specialization of classes where some but not all of the parameters are explicitly defined.   
- Can only be used with classes, not template functions.   
  - This is why a member function cannot be partially specialized.  
- For member functions, a solution is to use a partially specialized class.   
  - But this is very duplicate-heavy.   
- Another solution, we use inheritance.

26.6:

- Pointers don’t work with specialization.  
- Can create a partially specialized class with the pointer type.  
- This creates lifetime issues, because the internal pointer could be left dangling.   
  - Just fade all together and static assert that we aren’t passing in a pointer.   
  - Or use a unique pointer that automatically deallocates. 
