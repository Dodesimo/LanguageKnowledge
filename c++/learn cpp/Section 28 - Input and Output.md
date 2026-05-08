28.1:

- Input and output streams.  
  - Part of the io header.   
  - Uses multiple inheritance.  
- What is a stream: sequence of bytes that can be accessed sequentially.   
- Input stream: hold input from a data producer (keyboard, file, network).   
- Output streams: output for a particular data consumer, monitor, file, or printer.   
  - Essentially data is held in streams until used.  
- Istream: deals with input streams (\>\>), remove values from the stream.  
- Ostream: deals with output streams (\<\<) insertion operator is used to put values in the stream.   
- Iostream: handle input and output allowing bidirectional IO.   
- Standard stream: preconnected stream provided to a computer program by environment.   
  - Cin: istream tied to standard input (keyboard)  
  - Cout: ostream tried to standard output (monitor)  
  - Cerrr: tied to standard error (monitor), provides unbuffered output.  
  - Clog: tied to standard error (monitor), produces buffered output

28.2:

- How do you prevent buffer from overflowing?  
- Use a manipulator.  
  - Modify a stream when applied with \>\> or \<\<.  
  - Setw → limits the number of characters read from a stream.   
- Extraction operator skips whitespace (blanks, tabs, new lines).  
- .get() → allows you to read whitespace.   
  - Doesn ‘t read a newline character.   
- Get stops extraction when hitting a new line.   
- Getline → extract and discard delimiter.  
- gcount(). → how many characters were last extracted.   
- Other important functions.  
  - ignore(), discard the first character in the stream  
  - Ignore, discard a certain number of first characters  
  - Peek → read a character without removing it.  
  - Unget → return the last character read back into the stream  
  - Putback → put a character of choice back into the stream.

28.3:

- Two ways to change output formatting: flags and manipulators.  
- Flags: boolean variables turned on and off.  
- Manipulators: objects in a stream affecting the ways things are input and output.   
- Setf → put a flag.  
- Can combine flags with an OR.  
- Turn a flag off: use unsetf().  
- Format group: group of flags doing similar operations.  
  - If one is turned on, there’s precedence rules.  
- So unset one and then set the other.   
- Or use a version of setf where you pass in what you want to turn on and unset everything else.   
- Pass in a manipulator as well, which is just an object.   
- Ways to manipulate precision and number printing.  
  - Can set precision, if precision \< \# of significant digits, number will be rounded.  
- 

28.4:

- Stream classes for strings: allows us to do insertion and extraction operators on strings.  
- There’s a buffer, but not connected to an IO channel.  
- Six stream classes:  
  - Istringstream (istream)  
  - Ostringstream (ostream)  
  - Stringstream (iostream)  
- We can \<\< into the stringstream.  
- We can also use .str to set the value of the buffer.  
- Use os.str() to get the buffer results.   
- Can also use the extractio operator, but this will break on whitespaces.  
- Streams can be used really easily to convert integers to strings, vice versa.   
- Erase buffer with os.str(“”).  
  - Or use empty string std:;string{},  
- Reset error flags with clear().

28.5:

- State flags: tell us conditions about the stream.  
- Goodbit: everything is ok.  
- Badbit: fatal error has happened.  
- Eofbit: end of file.  
- Failbit: non fatal error occurred.  
- Member functions exist for this:  
  - good() → return true if set  
  - bad() → return true if set  
  - eof() → return true if set  
  - fail() → return true if set  
  - clear() → get rid of all flags  
  - clear(state) → clears all flags and set the state flag in.  
  - rdstate() → return all currently set flags  
  - setstate(state) → set the state flag passed in.  
- Failbit gets et when nothing can be extracted. 

28.6:

- File output: use the ofstream.   
- Output in C++ is buffered.   
  - So we don’t immediately write to disk, we batch operations.  
- Once a buffer is written to disk, known as flushing the buffer.  
- Cause a buffer to be flushed: close the file.   
- Data in the buffer, program terminates immediately. Destructors for the buffer are never closed so nothing ever gets flushed.   
- Std:;endl also flushes the buffer, but has a performance overhead.   
- To add files, you can add a second parameter to the the file stream constructor.  
  - App: append  
  - Ate: go to the end  
  - Binary: open in binary mode  
  - In: open in read mode  
  - Out: open the file in write mode  
  - Trunc: erase the file.   
  - Can manually call open and close.

28.7:

- Each file stream has a file pointer that keeps track of the current read/write position in the file.  
- When we read or write, happens at the file pointer’s current location.  
- By default, this is set to the beginning of the file.   
  - However if we are in append mode, the filewecan  pointer is moved to the end of the file.  
- We can do random access to the file at various points.   
  - Manipulate file pointer using seekg or seekp (offset, and from where).   
    - Beg: from start  
    - Cur: from the current location of file pointer  
    - End: relate to the end of the file.   
- Going to some place other than the beginning of file can have unexpected behavior.   
  - Newline is an abstraction: can take up two bytes of storage or 1 byte of storage.  
- Thus, lines ending with new lines will get different offset values.   
- Better used in binary files.  
- Tellg can tell us the size of a file.   
- To switch between an in and out, initialize fstream of with in or out.   
  - Then do a seekg to set pointer to where we are through tellg and beg offset. This swaps to read mode.   
- Don’t write pointers to disk because they change when prgrams are rerun. 