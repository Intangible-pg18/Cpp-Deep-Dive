# Cpp-Lesser-Known-Facts-

## BUFFER
Buffer-: A region of storage used to hold data. IO facilities often store input (or output) in a buffer and read or write the buffer independently from actions in the program. Using a
buffer allows the operating system to combine several output operations from our program into a single system-level write. Output buffers can be explicitly flushed to force the buffer
to be written. By default, reading cin flushes cout; cout is also flushed when the program ends normally. By default, All output buffers are flushed as part of the return from main.
Manipulator-: Object, such as std::endl, that when read or written “manipulates” the stream itself.
Clearing output buffer-: 
endl-: It is a manipulator. Writing endl has the effect of ending the current line and flushing the buffer associated with that device. Flushing the buffer ensures that all the output
the program has generated so far is actually written to the output stream, rather than sitting in memory waiting to be written.
flush-: writes the output, then flushes the buffer and adds no data (contrary to which endl, adds a new line).
ends-: writes the output and a null, then flushes the buffer.
Disadvantage-: Flushing of buffers is an Operating System task. Each time the buffer is flushed, a request has to be made to the OS and these requests are comparatively expensive. 
Furthermore, we don’t really need to flush the buffer every time we write something to the stream, since the buffers get flushed automatically when they get full. 
Solution-: Writing ‘\n’ characters directly to the stream is more efficient since it doesn’t force a flush like std::endl. 
Clearing input buffer-: 
In case of C++, after encountering“cin” statement, we require to input a character array or a string , we require to clear the input buffer or else the desired input is occupied by 
buffer of previous variable, not by the desired container.
Solution-: Typing “cin>>ws” after “cin” statement tells the compiler to ignore buffer and also to discard all the whitespaces before the actual content of string or character array. 
---






## STRINGS
In case of strings, >> ignores anything after whitespaces and getline() reads the whole line until it encounters a new line.
---
There is a threat of array decay in case of character array. As strings are represented as objects, no array decay occurs.
---
String methods-: getline()
                 push_back()
                 pop_back() 
                 capacity()
                 length()
                 resize()
                 copy(“char array”, len, pos)
                 Iterator functions-: begin()
                                      end()
                                      rbegin()
                                      rend()
---                                      
A stringstream associates a string object with a stream allowing you to read from the string as if it were a stream (like cin). 
---
Method-: operator << — add a string to the stringstream object
         operator >> — read something from the stringstream object
e.g. Count number of words in a string
                                      #include <bits/stdc++.h> 
                                      using namespace std; 
  
                                      int countWords(string str) 
                                      { 
                                       // breaking input into word using string stream 
                                       stringstream s(str); // Used for breaking words 
                                       string word; // to store individual words 
  
                                       int count = 0; 
                                       while (s >> word) 
                                         count++; 
                                       return count; 
                                      } 
                                      int main() 
                                      { 
                                       string s = "geeks for geeks geeks "
                                                  "contribution placements"; 
                                       cout << " Number of words are: " << countWords(s); 
                                       return 0; 
                                      } 
---





## ARRAYS
There is no index out of bounds checking in C++. But doing so may cause a Buffer Overflow (-: is an anomaly where a program, while writing data to a buffer, overruns the buffer's 
boundary and overwrites adjacent memory locations.)
---
Array name indicates the address of first element and arrays are always passed as pointers (even if we use square bracket).
---
Compiler uses pointer arithmetic to access array element. For example, an expression like “arr[i]” is treated as *(arr + i) by the compiler.
---
The loss of type and dimensions of an array is known as decay of an array.This generally occurs when we pass the array into function by value or pointer. 
---
What it does is, it sends first address to the array which is a pointer, hence the size of array is not the original one, but the one occupied by the pointer in the 
memory. A typical solution to handle decay is to send the array into functions by reference (int (&p)[7])
---
“array” is a “pointer to the first element of array” but “&array” is a “pointer to whole array of 5 int”. Since “array” is pointer to int, addition of 1 resulted in an address with 
increment of 4 (assuming int size in your machine is 4 bytes). Since “&array” is pointer to array of 5 ints, addition of 1 resulted in an address with increment of 4 x 5 = 20 = 0x14.
---



## FUNCTIONS
It is always recommended to declare a function before it is used. (int max(int, int); int *swap(int*,int);)
---
In C++, both void fun() and void fun(void) are same.
---
Once default value is used for an argument in function definition, all subsequent arguments to it must have default value.
---
When the program executes the function call instruction the CPU stores the memory address of the instruction following the function call, 
copies the arguments of the function on the stack and finally transfers control to the specified function. The CPU then executes the function code, 
stores the function return value in a predefined memory location/register and returns control to the calling function. This can become overhead if the execution time
of function is less than the switching time from the caller function to called function (callee). For functions that are large and/or perform complex tasks, the 
overhead of the function call is usually insignificant compared to the amount of time the function takes to run. However, for small, commonly-used functions, the 
time needed to make the function call is often a lot more than the time needed to actually execute the function’s code. This overhead occurs for small functions 
because execution time of small function is less than the switching time. C++ provides an inline functions to reduce the function call overhead.
---
Inlining is only a request to the compiler, not a command. Compiler can ignore the request for inlining. Compiler may not perform inlining in such circumstances like:
1) If a function contains a loop. (for, while, do-while)
2) If a function contains static variables.
3) If a function is recursive.
4) If a function return type is other than void, and the return statement doesn’t exist in function body.
5) If a function contains switch or goto statement.
---                                      
Disadvantage-: The added variables from the inlined function consumes additional registers, After in-lining function if variables number which are going to use 
               register increases than they may create overhead on register variable resource utilization.
               
               Too much inlining can also reduce your instruction cache hit rate, thus reducing the speed of instruction fetch from that of cache memory to that of 
               primary memory.
               
               Inline function may increase compile time overhead if someone changes the code inside the inline function then all the calling location has to be 
               recompiled because compiler would require to replace all the code once again to reflect the changes, otherwise it will continue with old functionality.
---                                     
All the functions defined inside the class are implicitly inline. 
---
Macro cannot access private members of class.
C++ compiler checks the argument types of inline functions and necessary conversions are performed correctly. Preprocessor macro is not capable for doing this.
Macros are managed by preprocessor and inline functions are managed by C++ compiler.
---                                      
C++ compiler cannot perform inlining if the function is virtual. The reason is call to a virtual function is resolved at runtime instead of compile time. Virtual 
means wait until runtime and inline means during compilation, if the compiler doesn’t know which function will be called, how it can perform inlining?                                      
---                                      
A void fun() can return another void function
---




## POINTERS & REFERENCES
* A pointer can be declared as void but a reference can never be void
* The pointer variable has n-levels/multiple levels of indirection i.e. single-pointer, double-pointer, triple-pointer. Whereas, the reference variable has only 
one/single level of indirection.
* Once a reference is created, it cannot be later made to reference another object; it cannot be reseated. This is often done with pointers. 
* References cannot be NULL. Pointers are often made NULL to indicate that they are not pointing to any valid thing. 
* A reference must be initialized when declared. There is no such restriction with pointers
there are few places like the copy constructor argument where pointer cannot be used. Reference must be used to pass the argument in the copy constructor. Similarly,
references must be used for overloading some operators like ++.
* Due to the above limitations, references in C++ cannot be used for implementing data structures like Linked List, Tree, etc. In Java, references don’t have the 
above restrictions and can be used to implement all data structures. References being more powerful in Java is the main reason Java doesn’t need pointers.
---



## MEMORY
A typical memory representation of C++ program consists of following sections.
1. Text segment (Code segment) -: Contains the compiled Machine code (program) instructions.
2. Initialized data segment (Data segment) -: Global and static variables
3. Uninitialized data segment (Block Started by Symbol (BSS) segment)
4. Heap (Free store) -: Used for Dynamic Memory allocation. C++ managed by new, delete.
5. Unallocated. Free area, available to be utilised for growth by heap or stack.
6. Stack -: Used for local variables and passing arguments to functions, along with return address of the next instruction to be executed when the function call is over. The stack is
            comprised of a number of Stack Frames, with each frame representing a function call.
7. OS -: Environment variables, command line arguments.
>>> https://cpp.tech-academy.co.uk/files/2013/01/memoryLayout5.png
>>> https://media.geeksforgeeks.org/wp-content/uploads/memoryLayoutC.jpg
>>> https://cpp.tech-academy.co.uk/files/2013/01/stackFrames.png
* In a stack, the allocation and deallocation is automatically done by whereas, in heap, it needs to be done by the programmer manually.
* Handling of Heap frame is costlier than handling of stack frame.
* Memory shortage problem is more likely to happen in stack whereas the main issue in heap memory is fragmentation.
* Stack frame access is easier than the heap frame as the stack have small region of memory and is cache friendly, but in case of heap frames which are dispersed 
throughout the memory so it cause more cache misses.
* Stack is not flexible, the memory size allotted cannot be changed whereas a heap is flexible, and the allotted memory can be altered.
Memory leakage occurs in C++ when programmers allocates memory by using new keyword and forgets to deallocate the memory by using delete() function or delete[] 
operator. Avoid-: 
* Instead of managing memory manually, try to use smart pointers where applicable.
* Use std::string instead of char *. The std::string class handles all memory management internally, and it’s fast and well-optimized.
* The best way to avoid memory leaks in C++ is to have as few new/delete calls at the program level as possible – ideally NONE. Anything that requires dynamic memory should be 
buried inside an RAII object that releases the memory when it goes out of scope. RAII allocate memory in constructor and release it in destructor, so that memory is garanteed to be 
deallocated when the variable leave the current scope.
---
