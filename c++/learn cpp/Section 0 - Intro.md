## 0.2

- Platform: compatible set of hardware and software.
- Provides useful services for the programs running on them. 
- Instruction set: possible list of machine language instructions written in binary (think like LC3). 
- Each CPU fam (x86, ARM64) has own machine language, not compatible with other CPU families. 
- Machine language → assembly language. 
- Assembler: Converts assembly language into machine language
- Machine language and assembly languages: not portable, since they are catered to machine hardware. 
- Compiler: takes source code and converts into machine code.
- This machine code then can be distributed to others.
- Doesn’t require a compiler to be installed to run the executable file. 
- Distributing compiled program doesn’t require knowledge of the source. Code. 
- Interpreter: directly executes instructions in the source code without requiring compilation. 
- Less efficient, interpretation is required every run. 
- Interpreter needs to be installed on every machine.  
- Benefits of high level languages: doesn’t require knowledge of underlying architecture. 

## 0.3

- C: efficient and flexible to the point where most unit operating systems were rewritten using it. 
- Most OS written in assembly, but produces programs that are CPU specific.
- C has excellent portability, allowing UNIX to be recompiled on various computers. 
- Major difference between C++ and C: C++ supports object oriented programing. 
- C++: standardized in 1998, five major updates after: C++11, C++14, C++17, C++20, C++23. 

## 0.4

- How do C++ programs get developed?
- Define a problem you would like to solve. 
- Determine how you are going to solve the problem. 
- Write the program. 
- C++ does not have any requirements for naming files. Defacto standard is to name the first/primary source file main.cpp. 	
- Can also have a .cc or .cxx extension. 

## 0.5

- Compiling source code: uses a C++ compiler. 
- Sequentially goes through each source code file, checks to see whether its in the rules of C++. Gives an error if it doesn’t. 
- Compiler translates C++ into machine language instructions, stored in an object file. 
- Name.o or name.obj.
- Linking object files and libraries and creating a desired output file. 
- Linker combines all the object files and produces a desired output file. 
- Ensures that all cross-file dependencies are resolved. 
- If defined something in one file, and used it in another, if not, get a linker error.
- Third, linker links in library files.
- Things you have imported or used.
- Then, outputs a correct executable. 
- Standard library: useful capabilities. 
- Most commonly used is input/output library. 
- Every C++ program utilizes this in some way, so most linkers are automatically configured to use this. 
- Building: the full process of converting source code into executables 
- Specific executable produced: called a build. 

## 0.7

- Build: compiles all modified files, links objects into an executable. 
- Clean: remove all cached objects and executables such that a new build results in a recompilation and a new executable produced.
- Rebuild: does clean and build.
- Compile: recompile a single code file, doesn’t invoke the linker or make an executable.
- Run/star: executes the executable from a prior build. 

## 0.9

- Build configuration: group of project settings determining how IDE builds project (what executable should be named, what directories, etc.)
- Debug configuration: debug your program, the one used when writing programs. 
- Turns off all optimizations and includes debugging information that makes programs slowed.
- Release configuration: used when releasing your program to the public: version is typically optimized for size and performance. 

## 0.10

- Most compilers follow rules defined in the C++ standard. 
- However, many compilers implement changes to enhance compatibility.
- Compiler extensions.
- Programs using non-standard C++ extensions will not compile on other compilers. 

## 0.11

- When there’s an issue, compiler emits a diagnostic.
- Modern compilers have adopted:
- Diagnostic error: compiler has halted compilation because it can’t proceed or the error is serious.
- Diagnostic warning: compiler has not decided to halt compilation, but proceeds.
- May not be the same for two compilers.
- Linker can also generate diagnostic errors if there’s an issue that cannot be resolved when linking. 

## 0.12

- Each C++ standard has a document that is the source for rules and requirements for a given language standard. 
- Used to develop compatible compilers. 