# Cpp- |Deep Dive| |Lesser-Known-Facts| & |Tips|

- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## BUFFER & STREAMS
Buffer-: A region of storage used to hold data. IO facilities often store input (or output) in a buffer and read or write the buffer independently from actions in the program. Using a buffer allows the operating system to combine several output operations from our program into a single system-level write. Output buffers can be explicitly flushed to force the buffer to be written. By default, reading cin flushes cout; cout is also flushed when the program ends normally. By default, All output buffers are flushed as part of the return from main.

---
Note-: cout is an object of ostream class and cin is an object istream class.

---
Manipulator-: Object, such as std::endl, that when read or written “manipulates” the stream itself.
Clearing output buffer-: 
* endl-: It is a manipulator. Writing endl has the effect of ending the current line and flushing the buffer associated with that device. Flushing the buffer ensures that all the output
the program has generated so far is actually written to the output stream, rather than sitting in memory waiting to be written.
* flush-: writes the output, then flushes the buffer and adds no data (contrary to which endl, adds a new line).
* ends-: writes the output and a null, then flushes the buffer.
Disadvantage-: Flushing of buffers is an Operating System task. Each time the buffer is flushed, a request has to be made to the OS and these requests are comparatively expensive. 
Furthermore, we don’t really need to flush the buffer every time we write something to the stream, since the buffers get flushed automatically when they get full. 
Solution-: Writing ‘\n’ characters directly to the stream is more efficient since it doesn’t force a flush like std::endl. 

---
Clearing input buffer-: 
In case of C++, after encountering“cin” statement, we require to input a character array or a string , we require to clear the input buffer or else the desired input is occupied by 
buffer of previous variable, not by the desired container.
Solution-: Typing “cin>>ws” after “cin” statement tells the compiler to ignore buffer and also to discard all the whitespaces before the actual content of string or character array. 

---

- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## MEMORY (UNDER THE HOOD)
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
* Note-: Each thread gets a stack, while there's typically only one heap for the application (although it isn't uncommon to have multiple heaps for different types of allocation).
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
Q-N-A -:
* To what extent are they (stack & heap) controlled by the OS or language runtime?
->The OS allocates the stack for each system-level thread when the thread is created. Typically the OS is called by the language runtime to allocate the heap for the application.
* What is their scope?
-> The stack is attached to a thread, so when the thread exits the stack is reclaimed. The heap is typically allocated at application startup by the runtime, and is reclaimed when the application (technically process) exits.
* What makes one faster?
-> The stack is faster because the access pattern makes it trivial to allocate and deallocate memory from it (a pointer/integer is simply incremented or decremented), while the heap has much more complex bookkeeping involved in an allocation or deallocation. Also, each byte in the stack tends to be reused very frequently which means it tends to be mapped to the processor's cache, making it very fast. Another performance hit for the heap is that the heap, being mostly a global resource, typically has to be multi-threading safe, i.e. each allocation and deallocation needs to be - typically - synchronized with "all" other heap accesses in the program. OR While the stack is allocated by the OS when the process starts (assuming the existence of an OS), it is maintained inline by the program. This is another reason the stack is faster, as well - push and pop operations are typically one machine instruction, and modern machines can do at least 3 of them in one cycle, whereas allocating or freeing heap involves calling into OS code. 
---
- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## FAULTS
They are signals generated when serious program error is detected by the operating system and there is no way the program could continue to execute because of these errors.
1. Core Dump or Segmentation fault (SIGSEGV) -: When a piece of code tries to do read and write operation in a read only location in memory or freed block of memory, it is known as 
   core dump.
Scenarios-:
  * Modifying a string literal (explained later)
  * Accessing an address that is freed
  * Accessing out of array index bounds (explained later)
  * Stack Overflow-:  It’s because of recursive function gets called repeatedly which eats up all the stack memory resulting in stack overflow. Running out of memory on the stack is 
    also a type of memory corruption. It can be resolved by having a base condition to return from the recursive function.
  * Dereferencing uninitialized pointer.
2. Bus Error (also known as SIGBUS and is usually signal 10) -:  occur when a process is trying to access memory that the CPU cannot physically address.In other words the memory 
   tried to access by the program is not a valid memory address.It caused due to alignment issues with the CPU (eg. trying to read a long from an address which isn’t a multiple of 
   4).
* Scenarios-: 
  * Program instructs the CPU to read or write a specific physical memory address which is not valid / Requested physical address is unrecognized by the whole computer system.
  * Unaligned access of memory (For example, if multi-byte accesses must be 16 bit-aligned, addresses (given in bytes) at 0, 2, 4, 6, and so on would be considered aligned and 
    therefore accessible, while addresses 1, 3, 5, and so on would be considered unaligned.)
The main difference between Segmentation Fault and Bus Error is that Segmentation Fault indicates an invalid access to a valid memory, while Bus Error indicates an access to an 
invalid address. 

---
- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## STRINGS
* In case of strings, >> ignores anything after whitespaces and getline() reads the whole line until it encounters a new line.
* There is a threat of array decay in case of character array. As strings are represented as objects, no array decay occurs.
* String methods-: getline()
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
* A stringstream associates a string object with a stream allowing you to read from the string as if it were a stream (like cin). 
* Method-: operator << — add a string to the stringstream object
           operator >> — read something from the stringstream object
e.g. Count number of words in a string
---
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
Storage for Strings in C++
Note-: When a string value is directly assigned to a pointer, in most of the compilers, it’s stored in a read-only block (generally in data segment) that is shared among functions.
e.g. char *str=  =  "GfG";                                                 
In the above line “GfG” is stored in a shared read-only location, but pointer str is stored in a read-write memory. You can change str to point something else but cannot change value at the address present at str i.e. str="mno" is possible but *str="nmo" is not. The former is a Segmentation Fault. (For more info jump to Faults section).

---
- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## ARRAYS
There is no index out of bounds checking in C++. But doing so may cause a Buffer Overflow (-: is an anomaly where a program, while writing data to a buffer, overruns the buffer's 
boundary and overwrites adjacent memory locations.) and a Segmentation Fault (-: For an array int arr[] = {1,2,3,4,5} , cout<<arr[10] works fine and prints a garbage value but arr[10]=9 results in a segmentation fault cause the memory beyond the buffer's boundaries in read-only memory.)
Array name indicates the address of first element and arrays are always passed as pointers (even if we use square bracket).
Compiler uses pointer arithmetic to access array element. For example, an expression like “arr[i]” is treated as *(arr + i) by the compiler.
The loss of type and dimensions of an array is known as decay of an array.This generally occurs when we pass the array into function by value or pointer. 
What it does is, it sends first address to the array which is a pointer, hence the size of array is not the original one, but the one occupied by the pointer in the 
memory. A typical solution to handle decay is to send the array into functions by reference (int (&p)[7])
“array” is a “pointer to the first element of array” but “&array” is a “pointer to whole array of 5 int”. Since “array” is pointer to int, addition of 1 resulted in an address with 
increment of 4 (assuming int size in your machine is 4 bytes). Since “&array” is pointer to array of 5 ints, addition of 1 resulted in an address with increment of 4 x 5 = 20 = 0x14.

---


- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
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
Disadvantage-: 
* The added variables from the inlined function consumes additional registers, After in-lining function if variables number which are going to use 
               register increases than they may create overhead on register variable resource utilization.
* Too much inlining can also reduce your instruction cache hit rate, thus reducing the speed of instruction fetch from that of cache memory to that of 
               primary memory.
* Inline function may increase compile time overhead if someone changes the code inside the inline function then all the calling location has to be 
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
Like friend class, a friend function can be given special grant to access private and protected members. A friend function can be:
a) A method of another class
b) A global function
class Node { 
private: 
    int key; 
    Node* next; 
    // Other members of Node Class
    friend int LinkedList::search(); 
    // Only search() of linkedList 
    // can access internal members 
}; 
Note-: Friendship is not mutual & Friendship is not inherited. . If a base class has a friend function, then the function doesn’t become a friend of the derived class(es).





- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## POINTERS & REFERENCES
* A pointer can be declared as void but a reference can never be void
* The pointer variable has n-levels/multiple levels of indirection i.e. single-pointer, double-pointer, triple-pointer. Whereas, the reference variable has only 
one/single level of indirection.
* Once a reference is created, it cannot be later made to reference another object; it cannot be reseated. This is often done with pointers. 
* References cannot be NULL. Pointers are often made NULL to indicate that they are not pointing to any valid thing. 
* A reference must be initialized when declared. There is no such restriction with pointers
there are few places like the copy constructor argument where pointer cannot be used. Reference must be used to pass the argument in the copy constructor. Similarly,
references must be used for overloading some operators like ++.
* A pointer has its own memory address and size on the stack whereas a reference shares the same memory address (with the original variable) but also takes up some space on the stack.
* Due to the above limitations, references in C++ cannot be used for implementing data structures like Linked List, Tree, etc. In Java, references don’t have the 
above restrictions and can be used to implement all data structures. References being more powerful in Java is the main reason Java doesn’t need pointers.
---
Types of pointers-: 
* Null Pointers
* Void Pointers
* Invalid Pointers
* Dangling Pointers
* This Pointers-: each object gets its own copy of data members and all objects share a single copy of member functions. Then now question is that if only one copy of each member function exists and is used by multiple objects, how are the proper data members are accessed and updated? The compiler supplies an implicit pointer along with the names of the functions as ‘this’. The ‘this’ pointer is passed as a hidden argument to all nonstatic member function calls and is available as a local variable within the body of all nonstatic functions. ‘this’ pointer is not available in static member functions as static member functions can be called without any object (with class name). For a class X, the type of this pointer is ‘X* ‘. Also, if a member function of X is declared as const, then the type of this pointer is ‘const X *’.
Uses-:
1. When local variable’s name is same as member’s name-: 
---
		       #include<iostream> 
                       using namespace std; 
                       class Test 
                       {     
                        int x; 
                       public: 
                        void setX (int x) 
                        { // The 'this' pointer is used to retrieve the object's x 
                          // hidden by the local variable 'x' 
                          this->x = x; 
                        } 
                        void print() { cout << "x = " << x << endl; } 
                        }; 
                       int main() 
                       { 
                        Test obj; 
                        int x = 20; 
                        obj.setX(x); 
                        obj.print(); 
                        return 0; 
                       } 
---
2) To return reference to the calling object-: Usecase-: Chain Function Calls
	---
		       #include<iostream> 
		       using namespace std; 
                       class Test 
                       { 
                        int x; 
                        int y; 
                        public: 
                         Test(int x = 0, int y = 0) { this->x = x; this->y = y; } 
                         Test &setX(int a) { x = a; return *this; } 
                         Test &setY(int b) { y = b; return *this; } 
                         void print() { cout << "x = " << x << " y = " << y << endl; } 
                         };  
                         int main() 
                         {  
                          Test obj1(5, 5); 
                          obj1.setX(10).setY(20); 
                          obj1.print(); 
                          return 0; 
                         }
	---
	
* Smart Pointers
* Wild Pointers
* Near, Far & Huge Pointers
* Opaque Pointers
---
Pointer to a function -: int * foo(int); operator precedence also plays role here ..so in this case, operator () will take priority over the asterisk operator . And the above declaration will mean – a function foo with one argument of int type and return value of int * i.e. integer pointer.
Solution-: int (* foo)(int); //there's no space between * and foo. Added space to avoid text formatting of a markdown file.

---
Note-: The Arrow(->) operator exists to access the members of the structure or the unions using pointers.

---



- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## Object Oriented Programming
When a class is defined, no memory is allocated but when it is instantiated (i.e. an object is created) memory is allocated. Object take up space in memory and have an associated address.

---
The standard does not permit objects (or classes) of size 0, this is because that would make it possible for two distinct objects to have the same memory location. This is the reason behind the concept that even an empty class must have a size at least 1. It is known that size of an empty class is not zero. Generally, it is 1 byte.
For dynamic allocation also, the new keyword returns different address for the same reason.
When a program is executed the objects interact by sending messages to one another. (For more info jump to *Message Passing* section)

---
By default the access modifier for the members will be Private.
Only the member functions or the friend functions are allowed to access the private data members of a class. 
Protected access modifier is similar to private access modifier in the sense that it can’t be accessed outside of it’s class unless with the help of friend class, the difference is that the class members declared as Protected can be accessed by any subclass(derived class) of that class as well. 
This access through inheritance can alter the access modifier of the elements of base class in derived class depending on the modes of Inheritance. (For more info jump to *Inheritance* section)
A friend class can access private and protected members of other class in which it is declared as friend. It is sometimes useful to allow a particular class to access private members of other class. For example a LinkedList class may be allowed to access private members of Node.
class Node { 
private: 
    int key; 
    Node* next; 
    // Other members of Node Class 
    // Now class  LinkedList can 
    // access private members of Node 
    friend class LinkedList; 
}; 
---
Note-: Friendship is not mutual & Friendship is not inherited. . If a base class has a friend function, then the function doesn’t become a friend of the derived class(es).

---
Note-: Like member functions and member function arguments, the objects of a class can also be declared as const. an object declared as const cannot be modified and 
       hence, can invoke only const member functions as these functions ensure not to modify the object.
Constructors-: 
* They can be defined in private section of class and thus can only be used by a friend class.

---
Types of Contructors-: 
* Default Constructor -: Consider a class derived from another class with the default constructor, or a class containing another class object with default constructor. The compiler 
                         needs to insert code to call the default constructors of base class.
* Parameterized Constructor
* Copy Constructor -:  is a member function which initializes an object using another object of the same class.

---
                       **OBJECT COPYING**
		       It is the process of creating a copy of an existing object. Copying is done mostly so the copy can be modified or moved, or the 
                       current value preserved. If either of these is unneeded, a reference to the original data is sufficient and more efficient, as no copying occurs. 
                       Types-: 
		       1. Shallow copy-: In that case a new object B is created, and the fields values of A are copied over to B. If the field value is a primitive type it copies the 
		       value of the primitive type. In languages without primitive types (where everything is an object), all fields of the copy B are references to the same objects 
		       as the fields of original A. https://media.geeksforgeeks.org/wp-content/uploads/copy-constructor.png
		       2. Deep copy-: Rather than references to objects being copied, new copy objects are created for any referenced objects, and references to these placed in B.  
		       The result is different from the result a shallow copy gives in that the objects referenced by the copy B are distinct from those referenced by A, and 
		       independent. https://media.geeksforgeeks.org/wp-content/uploads/copy-constructor1.png
		       3. Lazy copy-: A lazy copy is an implementation of a deep copy. When initially copying an object, a (fast) shallow copy is used. A counter is also used to 
		       track how many objects share the data. When the program wants to modify an object, it can determine if the data is shared (by examining the counter) and can do 
		       a deep copy if needed. Lazy copy looks to the outside just as a deep copy, but takes advantage of the speed of a shallow copy whenever possible.

---
                       **Compiler's Optimizations**
		       1. Copy Elision-: or Copy omission is a compiler optimization technique that avoids unnecessary copying of objects.
		          e.g. For  B ob = "copy me"; // B is a class
			       According to theory, when the object “ob” is being constructed, one argument constructor is used to convert “copy me” to a temporary object & that   
			       temporary object is copied to the object “ob”. But now the call to the copy constructor is elided.
		       2. Return Value Optimization-: is a compiler optimization that involves eliminating the temporary object created to hold a function's return value.

---
		       #include<iostream> 
                       using namespace std; 
                       class Point 
                        { 
                         private: 
	                               int x, y; 
                         public: 
	                              Point(int x1, int y1) { x = x1; y = y1; } 
	                              Point(const Point& p1) {x = p1.x; y = p1.y; } 
                                int getX()		 { return x; } 
                                int getY()		 { return y; } 
                        }; 
                        int main() 
                        { 
	                       Point p1(10, 15); // Normal constructor is called here 
	                       Point p2=p1; // Copy constructor is called here 
	                       cout << "p1.x = " << p1.getX() << ", p1.y = " << p1.getY(); 
	                       cout << "\np2.x = " << p2.getX() << ", p2.y = " << p2.getY(); 
                         return 0; 
                         }
  ---
                         A copy constructor is called when an object is passed by value. Copy constructor itself is a function. So if we pass an argument by value in a copy 
			 constructor, a call to copy constructor would be made to call copy constructor which becomes a non-terminating chain of calls. Therefore compiler doesn’t 
			 allow parameters to be passed by value.		 
---			 
                         Why const?
			 #include<iostream> 
                         using namespace std; 
                         class Test 
                         { 
                          /* Class data members */
                         public: 
                         Test(Test &t) { /* Copy data members from t*/} 
                         Test()	 { /* Initialize data members */ } 
                         }; 
                         Test fun() 
                         { 
	                   cout << "fun() Called\n"; 
	                   Test t; 
	                   return t; 
                         } 
                         int main() 
                         { 
	                  Test t1; 
	                  Test t2 = fun(); 
	                  return 0; 
                         } 
	                 >>>Compile time error. Reason-: The function fun() returns by value. So the compiler creates a temporary object which is copied to t2 using copy constructor 
	                 in the original program (The temporary object is passed as an argument to copy constructor). The reason for compiler error is, compiler created temporary 
	                 objects cannot be bound to non-const references because it doesn’t make sense to modify compiler created temporary objects as they can die any moment. 
                         Solution-: 1. Use const. 2. Test t2; t2 = fun(); 
---
                         In C++, a Copy Constructor may be called in following cases:
                         1. When an object of the class is returned by value.
                         2. When an object of the class is passed (to a function) by value as an argument.
                         3. When an object is constructed based on another object of the same class.
                         4. When the compiler generates a temporary object.
                         
  ---
                         It is, however, not guaranteed that a copy constructor will be called in all these cases, because the C++ Standard allows the compiler to optimize the copy                                
                         away in certain cases, one example is the return value optimization (sometimes referred to as RVO)
		
---
                         Note-: In C++ computer programming, copy elision refers to a compiler optimization technique that eliminates unnecessary copying of objects. The C++ language                   
                         standard generally allows implementations to perform any optimization, provided the resulting program's observable behavior is the same as if, i.e. 
                         pretending, the program were executed exactly as mandated by the standard.
                         In the context of the C++ programming language, return value optimization (RVO) is a compiler optimization that involves eliminating the temporary object 
                         created to hold a function's return value.The standard also describes a few situations where copying can be eliminated even if this would alter the program's  
                         behavior, the most common being the return value optimization.
			 
---
                         If we don’t define our own copy constructor, the C++ compiler creates a default copy constructor for each class which does a member-wise copy between 
			 objects. The compiler created copy constructor works fine in general. We need to define our own copy constructor only if an object has pointers or any 
			 runtime allocation of the resource like file handle, a network connection..etc. Default copy constructor does only shallow copy. Deep copy is possible only 
			 with user defined copy constructor. 
			 
---
                         Copy constructor is called when a new object is created from an existing object, as a copy of the existing object (see this G-Fact). And assignment operator 
			 is called when an already initialized object is assigned a new value from another existing object. 
* Conversion Constructor
* Explicit Constructor
* Dynamic Constructor
* Dummy Constructor
* Move Constructor

---
Destructor-:
* When something is created using dynamic memory allocation, it is programmer’s responsibility to delete it. So compiler doesn’t bother. 
* When a class has private destructor, only dynamic objects of that class can be created and use a function as friend of the class to call delete().
* It cannot be declared static or const.
* An object of a class with a Destructor cannot become a member of the union.
* The programmer cannot access the address of destructor.
* There can only one destructor in a class
* It is always a good idea to make destructors virtual in base class when we have a virtual function.

---
Types of Destructors-:
* Virtual Destructor-: Discussed in the *Virtual Function & Runtime Polymorphism (Function Overriding)* section.
* Pure Destructor

---
Whenever we define one or more non-default constructors( with parameters ) for a class, a default constructor( without parameters ) should also be explicitly defined as the compiler will not provide a default constructor in this case.

---














- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## ABSTRACTION
Types-: 
* Abstraction using Classes: We can implement Abstraction in C++ using classes. The class helps us to group data members and member functions using available access specifiers. A Class can decide which data member will be visible to the outside world and which is not.
* Abstraction in Header files: One more type of abstraction in C++ can be header files. For example, consider the pow() method present in math.h header file. Whenever we need to calculate the power of a number, we simply call the function pow() present in the math.h header file and pass the numbers as arguments without knowing the underlying algorithm according to which the function is actually calculating the power of numbers.

---
- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## Inheritance
Modes Of Inheritance-:
* Public mode: If we derive a sub class from a public base class. Then the public member of the base class will become public in the derived class and protected members of the base 
  class will become protected in derived class.
* Protected mode: If we derive a sub class from a Protected base class. Then both public member and protected members of the base class will become protected in derived class.
* Private mode: If we derive a sub class from a Private base class. Then both public member and protected members of the base class will become Private in derived class.

---
Note : The private members in the base class cannot be directly accessed in the derived class, while protected members can be directly accessed. 

---
Constructor Calls in Inheritance-:
Base class constructors are always called in the derived class constructors. Whenever you create derived class object, first the base class default constructor is executed and then 
the derived class's constructor finishes execution.

---
Points to Remember-:
* Whether derived class's default constructor is called or parameterised is called, base class's default constructor is always called inside them.
* To call base class's parameterised constructor inside derived class's parameterised constructo, we must mention it explicitly while declaring derived class's parameterized 
  constructor. 
*                          #include<iostream> 
                           using namespace std; 
                           class Base
                           {   int x;
                               public:
                               // parameterized constructor
                               Base(int i)
                               {  x = i;
                                  cout << "Base Parameterized Constructor\n";
                               }
                           };
                           class Derived : public Base
                           {   int y;
                               public:
                               // parameterized constructor
                               Derived(int j):Base(j)
                               { y = j;
                                 cout << "Derived Parameterized Constructor\n";
                               }
                           };
                           int main()
                           {  Derived d(10) ;
                           }
* Why is Base class Constructor called inside Derived class? -> Constructors have a special job of initializing the object properly. A Derived class constructor has access only to 
	its own class members, but a Derived class object also have inherited property of Base class, and only base class constructor can properly initialize base class members. 
	Hence all the constructors are called, else object wouldn't be constructed properly.	



---
* Note-: 
  1. Constructors and Destructors are never inherited and hence never overrided.(We will study the concept of function overriding in the next tutorial)
  2. Assignment operator = is never inherited. It can be overloaded but can't be inherited by sub class.
* Inheritance and Static Functions in C++ -:
  1. They are inherited into the derived class.
  2. If you redefine a static member function in derived class, all the other overloaded functions in base class are hidden.
  3. Static Member functions can never be virtual. We will study about Virtual in coming topics.
---
Types of Inheritance-:
* Single Inheritance: In single inheritance, a class is allowed to inherit from only one class. i.e. one sub class is inherited by one base class only.    
  https://media.geeksforgeeks.org/wp-content/uploads/single-inheritance.png
* Multiple Inheritance (V): Multiple Inheritance is a feature of C++ where a class can inherit from more than one classes. i.e one sub class is inherited from more than one base classes.
  https://media.geeksforgeeks.org/wp-content/uploads/multiple-inheritance.png Note-: The constructors of inherited classes are called in the same order in which they are inherited   
  and the destructors are called in reverse order of constructors. 
* Multilevel Inheritance-: In this type of inheritance, a derived class is created from another derived class. https://media.geeksforgeeks.org/wp-content/uploads/multilevel-inheritance.png  
* Hierarchical Inheritance (A)-: In this type of inheritance, more than one sub class is inherited from a single base class. i.e. more than one derived class is created from a single base class. https://media.geeksforgeeks.org/wp-content/uploads/hierarchical-inheritance.png
* Hybrid (Virtual) Inheritance: Hybrid Inheritance is implemented by combining more than one type of inheritance. For example: Combining Hierarchical inheritance and Multiple Inheritance. https://media.geeksforgeeks.org/wp-content/uploads/Hybrid-Inheritance.png
---
- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## Inheritance -> UPCASTING & DOWNCASTING
* Upcasting -: is a process of creating a pointer or a reference of the derived class object as a base class pointer. You do not need to upcast manually. You just need to assign 
derived class pointer (or reference) to base class pointer.
* When you use upcasting, the object is not changing. Nevertheless, when you upcast an object, you will be able to access only member functions and data members that are defined in 
the base class.
* One of the biggest advantages of upcasting is the capability of writing generic functions for all the classes that are derived from the same base class.

---
                         #include <iostream>
                         using namespace std;
                         class Person
                           {
                             //content of Person
                           };
                           class Employee:public Person
                           {
                           public:
                             Employee(string fName, string lName, double sal)
                             {
                               FirstName = fName;
                               LastName = lName;
                               salary = sal;
                             }
                             string FirstName;
                             string LastName;
                             double salary;
                             void show()
                             {
                               cout << "First Name: " << FirstName << " Last Name: " << LastName << " Salary: " << salary<< endl;
                             }
                             void addBonus(double bonus)
                             {
                               salary += bonus;
                             }
                          };
                           class Manager :public Employee
                                                    {
                           public:
                             Manager(string fName, string lName, double sal, double comm) :Employee(fName, lName, sal)
                             {
                               Commision = comm;
                             }
                             double Commision;
                             double getComm()
                             {
                               return Commision;
                             }
                           };
                           class Clerk :public Employee
                           {
                           public:
                             Clerk(string fName, string lName, double sal, Manager* man) :Employee(fName, lName, sal)
                             {
                               manager = man;
                             }
                             Manager* manager;
                             Manager* getManager()
                             {
                              return manager;
                             }
                           };
                           void congratulate(Employee* emp) //generic function
                           {
                             cout << "Happy Birthday!!!" << endl;
                             emp->addBonus(200);
                             emp->show();
                           };
                           int main()
                           {
                               //pointer to base class object
                               Employee* emp;
                               //object of derived class
                               Manager m1("Steve", "Kent", 3000, 0.2);
                               Clerk c1("Kevin","Jones", 1000, &m1);
                               //implicit upcasting
                               emp = &m1; 
                               //It's ok
                              cout<<emp->FirstName<<endl;
                               cout<<emp->salary<<endl;
                               //Fails because upcasting is used
                               //cout<<emp->getComm();
                               congratulate(&c1);
                               congratulate(&m1);    
                               cout<<"Manager of "<<c1.FirstName<<" is "<<c1.getManager()->FirstName;
                           }
                         >>Steve
                           3000
                           Happy Birthday!!!
                           First Name: Kevin Last Name: Jones Salary: 1200
                           Happy Birthday!!!
                           First Name: Steve Last Name: Kent Salary: 3200
                           Manager of Kevin is Steve
	
---	
* Memory Layout-: https://www.tutorialcup.com/images/cplusplus/upcasting-downcasting/upcasting-memory-layout.png  -> It represents the fact that when you use a base class pointer to 
point up an object of the derived class, you can access only elements that are defined in the base class (green area). Elements of the derived class (yellow area) are not accessible 
when you use a base class pointer.
* Downcasting is an opposite process for upcasting. It converts base class pointer to derived class pointer. Downcasting must be done manually. It means that you have to specify 
explicit typecast.
* Downcasting is not as safe as upcasting. You know that a derived class object can be always treated as a base class object. However, the opposite is not right.
* You have to use an explicit cast for downcasting: 

---
*                          //pointer to base class object
                           Employee* emp;
                           //object of derived class
                           Manager m1("Steve", "Kent", 3000, 0.2);
                           //implicit upcasting
                           emp = &m1;
                           //explicit downcasting from Employee to Manager
                           Manager* m2 = (Manager*)(emp);
                           >>>This code compiles and runs without any problem because emp points to an object of Manager class.

---
*                          But, 
                           Employee e1("Peter", "Green", 1400);
                           //try to cast an employee to Manager
                           Manager* m3 = (Manager*)(&e1);
                           cout << m3->getComm() << endl;
                           >>>e1 object is not an object of the Manager class. It does not contain any information about the commission. That’s why such an operation can produce 
			   unexpected results.
* Memory layout-:https://www.tutorialcup.com/images/cplusplus/upcasting-downcasting/upcasting-memory-layout.png  -> When you try to downcast base class pointer (Employee) that is not 
actually pointing up an object of the derived class (Manager), you will get access to the memory that does not have any information about the derived class object (yellow area). This 
is the main danger of downcasting. You can use a safe cast that can help you to know if one type can be converted correctly to another type. For this purpose, use a dynamic cast.
* Dynamic_Cast-: is an operator that converts safely one type to another type. In the case, the conversation is possible and safe, it returns the address of the object that is 
converted. Otherwise, it returns nullptr.
* If you want to use a dynamic cast for downcasting, the base class should be polymorphic – it must have at least one virtual function. Modify base class Person by adding a virtual function:
* virtual void foo() {}
*                           Employee e1("Peter", "Green", 1400);
                            Manager* m3 = dynamic_cast<Manager*>(&e1);
                            if (m3)
                            cout << m3->getComm() << endl;
                            else
                            cout << "Can't  cast from Employee to Manager" << endl;
* In this case, the dynamic cast returns nullptr. Therefore, you will see a warning message.

---
- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## Polymorphism
Types of polymorphism-: https://media.geeksforgeeks.org/wp-content/uploads/20200703160531/Polymorphism-in-CPP.png

---
* Binding-: refers to the process of converting identifiers (such as variable and function names) into addresses. Binding is done for each variable and functions. For functions, it means that matching the call with the right function definition by the compiler. It takes place either at compile time or at runtime.
* Early Binding (compile-time time polymorphism or static binding) As the name indicates, compiler (or linker) directly associate an address to the function call. It replaces 
the call with a machine language instruction that tells the mainframe to leap to the address of the function. By default early binding happens in C++.
1. Operator Overloading-:
Note-: 1. Most overloaded operators may be defined as ordinary non-member functions or as class member functions. Box operator+(const Box&);
       In case we define above function as non-member function of a class then we would have to pass two arguments for each operand as Box operator+(const Box&, const 
       Box&);
       2. Operators which cannot be overloaded-:     ?: (conditional)
                                                     . (member selection)
                                                     .* (member selection with pointer-to-member)
                                                     :: (scope resolution)
                                                     sizeof (object size information)
                                                     typeid (object type information)
                                                     static_cast (casting operator)
                                                     const_cast (casting operator)
                                                     reinterpret_cast (casting operator)
                                                     dynamic_cast (casting operator)
       3. For operator overloading to work, at least one of the operands must be a user defined class object.
       4. Overloaded operators cannot have default arguments except the function call operator () which can have default arguments.
       5. Compiler automatically creates a default assignment operator with every class. 
       6. Assignment (=), subscript ([]), function call (“()”), and member selection (->) operators must be defined as member functions, all other operators can be 
          either member functions or a non member functions.
       7. Some operators like (assignment)=, (address)& and comma (,) are by default overloaded.
       8. Consider the statement “ob1 + ob2” (let ob1 and ob2 be objects of two different classes). To make this statement compile, we must overload ‘+’ in class of ‘ob1’ or 
          make ‘+’ a global function. Which can be quite inconvenient in some cases (e.g. <<,>>).

---
Overloading the Output Operator <<
*  Ordinarily, the first parameter of an output operator is a reference to a nonconst ostream object. The ostream is nonconst because writing to the stream changes its state.  
The parameter is a reference because we cannot copy an ostream object. The second parameter ordinarily should be a reference to const of the class type we want to print. The 
parameter is a reference to avoid copying the argument. It can be const because (ordinarily) printing an object does not change that object.
*                  void operator<<(ostream &os, const book &item)
                   {
                   os << item.isbn() << " " << item.units_sold << " "
                   << item.revenue << " " << item.avg_price();
                   }
		   ---
Note-: Differentiating Prefix and Postfix Operators-: Normal overloading cannot distinguish between these operators. The prefix and postfix versions use the same symbol, 
meaning  that the overloaded versions of these operators have the same name. They also have the same number and type of operands. To solve this problem, the postfix versions 
take an extra (unused) parameter of type int. When we use a postfix operator, the compiler supplies 0 as the argument for this parameter.

---
Function Overloading-: 
* Parameter declarations that differ only in a pointer * versus an array [] are equivalent. That is, the array declaration is adjusted to become a pointer declaration. Only 
 the second and subsequent array dimensions are significant in parameter types. For example, following two function declarations are equivalent
 int fun(int * ptr); 
 int fun(int ptr[]); // redeclaration of fun(int * ptr)
 
 ---
 - ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## Virtual Function & Runtime Polymorphism (Function Overriding)
* Late Binding (Run time polymorphism or dynamic binding)-: In this, the compiler adds code that identifies the kind of object at runtime then matches the call with the right function definition.
* A Base Class pointer can point to a derived class object. Why is the vice-versa not true? ->  A derived class contains the properties of it's super class but the super class doesn't contain the properties of the derived class. A derived object is a base class object (as it's a sub class), so it can be pointed to by a base class pointer. However, a base class object is not a derived class object so it can't be assigned to a derived class pointer.
* A base class pointer can point to a derived class object, but we can only access base class member or virtual functions using the base class pointer because object slicing happens when a derived class object is assigned to a base class object. Additional attributes of a derived class object are sliced off to form the base class object.
* Example of the above point-: A class base contains the following function definition-: virtual void fun_4() { cout << "base-4\n"; } and the derived class conatins the following: void fun_4(int x) { cout << "derived-4\n"; }. Then doing this-: base* p; derived obj1; p = &obj1; p->fun_4(5); is illegal.
* Object Slicing-: happens when a derived class object is assigned to a base class object, additional attributes of a derived class object are sliced off to form the base class object.
---
                                         #include <iostream> 
                                         using namespace std; 
                                         class Base 
                                         { 
                                         protected: 
	                                         int i; 
                                         public: 
                                         Base(int a)	 { i = a; } 
                                         virtual void display() 
                                         { cout << "I am Base class object, i = " << i << endl; } 
                                         }; 
                                         class Derived : public Base 
                                         { 
	                                         int j; 
                                         public: 
	                                         Derived(int a, int b) : Base(a) { j = b; } 
	                                         virtual void display() 
	                                         { cout << "I am Derived class object, i = "
		                                         << i << ", j = " << j << endl; } 
                                         }; 
                                         void somefunc (Base obj) 
                                         { 
	                                         obj.display(); 
                                         } 
                                         int main() 
                                         { 
	                                         Base b(33); 
	                                         Derived d(45, 54); 
	                                         somefunc(b); 
	                                         somefunc(d); // Object Slicing, the member j of d is sliced off 
	                                         return 0; 
                                         }
	---
* I am Base class object, i = 33
  I am Base class object, i = 45
* Solution 1-: Object slicing doesn’t occur when pointers or references to objects are passed as function arguments since a pointer or reference of any type takes same amount of memory and prevents copying.  -> void somefunc (Base &obj)
* Solution 2-: Object slicing can be prevented by making the base class function pure virtual there by disallowing object creation. It is not possible to create the object of a class which contains a pure virtual method.
* A virtual function is a member function that you expect to be redefined in derived classes. When you refer to a derived class object using a pointer or a reference to the base class, you can call a virtual function for that object and execute the derived class's version of the function.
---
                                         #include <iostream>
                                         using namespace std;
                                         class Account {
                                         public:
                                            Account( double d ) { _balance = d; }
                                            virtual double GetBalance() { return _balance; }
                                            virtual void PrintBalance() { cerr << "Error. Balance not available for base type." << endl; }
                                         private:
                                             double _balance;
                                         };
                                         class CheckingAccount : public Account {
                                         public:
                                            CheckingAccount(double d) : Account(d) {}
                                            void PrintBalance() { cout << "Checking account balance: " << GetBalance() << endl; }
                                         };
                                         class SavingsAccount : public Account {
                                         public:
                                            SavingsAccount(double d) : Account(d) {}
                                            void PrintBalance() { cout << "Savings account balance: " << GetBalance(); }
                                         };
                                         int main() {
                                            CheckingAccount checking( 100.00 );
                                            SavingsAccount  savings( 1000.00 );
                                            Account *pAccount = &checking;
                                            pAccount->PrintBalance();
                                            pAccount = &savings;
                                            pAccount->PrintBalance();
                                         }	
	---
* In the preceding code, the calls to PrintBalance are identical, except for the object pAccount points to. Because PrintBalance is virtual, the version of the function defined for each object is called. The PrintBalance function in the derived classes CheckingAccount and SavingsAccount "override" the function in the base class Account.In the preceding code, the calls to PrintBalance are identical, except for the object pAccount points to. Because PrintBalance is virtual, the version of the function defined for each object is called. The PrintBalance function in the derived classes CheckingAccount and SavingsAccount "override" the function in the base class Account.
* A function that is virtual in a base class is implicitly virtual in its derived classes-: When a derived class overrides a virtual function, it may, but is not required to,  repeat the virtual keyword. Once a function is declared as virtual, it remains virtual in all the derived classes.
* If a class is declared that does not provide an overriding implementation of the PrintBalance function, the default implementation from the base class Account is used.
* Rules-: 1. A call to a virtual function is resolved according to the underlying type of object for which it is called.
	  2. A call to a nonvirtual function is resolved according to the type of the pointer or reference.
	  3. Because virtual functions are called only for objects of class types, you cannot declare global or static functions as virtual.
	  4. A class may have virtual destructor but it cannot have a virtual constructor. (More details later)
* Use-: Virtual functions allow us to call methods of any of the derived classes without even knowing kind of derived class object. 
* Usecase-: To raise the salary of each employee in the organisation-:
	
	---
                                      class Employee {
                                      public:
	                                      virtual void raiseSalary()
	                                      {
		                                      /* common raise salary code */
	                                      }
	                                      virtual void promote() { /* common promote code */ }
                                      };
                                      class Manager : public Employee {
	                                      virtual void raiseSalary()
	                                      {
		                                      /* Manager specific raise salary code, may contain
		                                      increment of manager specific incentives*/
	                                      }
	                                      virtual void promote()
	                                      {
		                                      /* Manager specific promote */
	                                      }
                                      };
                                      // Similarly, there may be other types of employees                                      
                                      // We need a very simple function
                                      // to increment the salary of all employees
                                      // Note that emp[] is an array of pointers
                                      // and actual pointed objects can
                                      // be any type of employees.
                                      // This function should ideally
                                      // be in a class like Organization,
                                      // we have made it global to keep things simple
                                      void globalRaiseSalary(Employee* emp[], int n)
                                      {
	                                      for (int i = 0; i < n; i++
		                                      // Polymorphic Call: Calls raiseSalary()
		                                      // according to the actual object, not
		                                      // according to the type of pointer
		                                      emp[i]->raiseSalary();
                                      }
	---
*Working of the above code and the concept of VTABLE and VPTR-: 
* vtable: A table of function pointers, maintained per class and created at compile time.
* vptr: A pointer to vtable, maintained per object instance and is inherited by derived classes.
* If a class contains a virtual function then compiler itself does two things: 1. If object of that class is created then a virtual pointer (VPTR) is inserted as a data member of the class to point to virtual table (VTABLE) of that class. For each new object created, a new virtual pointer is inserted as a data member of that class. 2. Irrespective of object is created or not, a static array of function pointer called VTABLE where each cell contains the address of each virtual function contained in that class.
* For the code above this is what happens under the hood-: https://media.geeksforgeeks.org/wp-content/uploads/VirtualFunction.png
* Compiler adds additional code at two places to maintain and use vptr-: 
1) Code in every constructor. This code sets the vptr of the object being created. This code sets vptr to point to the vtable of the class. 
2) Wherever a polymorphic call is made, the compiler inserts code to first look for vptr using base class pointer or reference (In the above example, since pointed or referred object is of derived type, vptr of derived class is accessed). Once vptr is fetched, vtable of derived class can be accessed. Using vtable, address of derived class function raisesalary() is accessed and called.
* Example-:
---
                                      #include <iostream> 
                                      using namespace std; 
                                      class A 
                                      { 
                                      public: 
                                          virtual void fun(); 
                                      }; 
                                      class B 
                                      { 
                                      public: 
                                         void fun(); 
                                      }; 
                                      int main() 
                                      { 
                                          int a = sizeof(A), b = sizeof(B); 
                                          if (a == b) cout << "a == b"; 
                                          else if (a > b) cout << "a > b"; 
                                          else cout << "a < b"; 
                                          return 0; 
                                      }
						  ---
* Output -> a > b
* Explanation-: Class A has a VPTR which is not there in class B. In a typical implementation of virtual functions, compiler places a VPTR with every object. Compiler secretly adds some code in every constructor to this.
* Note-: By default all the functions defined inside the class are implicitly or automatically considered as inline except virtual functions. Whenever virtual function is called using base class reference or pointer it cannot be inlined (because call is resolved at runtime), but whenever called using the object (without reference or pointer) of that class, can be inlined because compiler knows the exact class of the object at compile time.	
* Deleting a derived class object using a pointer of base class type that has a non-virtual destructor results in undefined behavior. To correct this situation, the base class should be defined with a virtual destructor. Making base class destructor virtual guarantees that the object of derived class is destructed properly, i.e., both base class and derived class destructors are called. Any time you have a virtual function in a class, you should immediately add a virtual destructor (even if it does nothing).
* Pure Virtual Functions & Abstract Classes-: Sometimes implementation of all function cannot be provided in a base class because we don’t know the implementation. Such a class is called abstract class. For example, let Shape be a base class. We cannot provide implementation of function draw() in Shape, but we know every derived class must have implementation of draw(). A pure virtual function (or abstract function) in C++ is a virtual function for which we don’t have implementation, we only declare it. A pure virtual function is declared by assigning 0 in declaration. 
						  ---
*                                      // An abstract class 
                                      class Test 
                                      {    
                                          // Data members of class 
                                      public: 
                                          // Pure Virtual Function 
                                          virtual void show() = 0;     
                                      }; 
						  ---
* Note-: 1. A class is abstract if it has at least one pure virtual function.
	 2. If we do not override the pure virtual function in derived class, then derived class also becomes abstract class.
	 3. We cannot create objects of abstract classes. Reasoon-: Because it's abstract and an object is concrete. An abstract class is sort of like a template, or an empty/partially empty structure, you have to extend it and build on it before you can use it. Take for example an "Animal" abstract class. There's no such thing as a "pure" animal - there are specific types of animals. So you can instantiate Dog and Cat and Turtle, but you shouldn't be able to instantiate plain Animal - that's just a basic template. And there's certain functionality that all animals share, such as "makeSound()", but that can't be defined on the base Animal level. So if you could create an Animal object and you would call makeSound(), how would the object know which sound to make?
         4. An abstract class can have constructors. Reason-: An abstract class can have member variables and potentially non-virtual member functions, so that every derived class from the former implements specific features. Then, the responsibility for the initialization of these members variables may belong to the abstract class (at least always for private members, because the derived class wouldn't be able to initialize them, yet could use some inherited member functions that may use/rely on these members). Thus, it makes it perfectly reasonable for abstract classes to implement constructors.
	 5. We can have pointers and references of abstract class type (But they should not be pointing at the astract class's object but to any of the derived class's object.
						  ---
* Multiple Inheritace and Hybrid Inheritance -> Virtual Inheritance (The Diamond Problem) -> Virtual Base Class -: The diamond problem occurs when two superclasses of a class have a common base class. For example, in the following diagram, the TA class gets two copies of all attributes of Person class, this causes ambiguities. https://media.geeksforgeeks.org/wp-content/uploads/diamondproblem.png						
						---
                         #include<iostream> 
                         using namespace std; 
                         class Person { 
                         // Data members of person 
                         public: 
                         	Person(int x) { cout << "Person::Person(int ) called" << endl; } 
                         }; 
                         class Faculty : public Person { 
                         // data members of Faculty 
                         public: 
	                         Faculty(int x):Person(x) { 
	                         cout<<"Faculty::Faculty(int ) called"<< endl; 
	                         } 
                         }; 
                         class Student : public Person { 
                         // data members of Student 
                         public: 
	                         Student(int x):Person(x) { 
		                         cout<<"Student::Student(int ) called"<< endl; 
	                         } 
                         }; 
                         class TA : public Faculty, public Student { 
                         public: 
	                         TA(int x):Student(x), Faculty(x) { 
		                         cout<<"TA::TA(int ) called"<< endl; 
	                         } 
                         }; 
                         int main() { 
	                         TA ta1(30); 
                         } 
	---
	                 Person::Person(int ) called
   	                 Faculty::Faculty(int ) called
   	                 Person::Person(int ) called
   	                 Student::Student(int ) called
   	                 TA::TA(int ) called
	---
* In the above program, constructor of ‘Person’ is called two times. Destructor of ‘Person’ will also be called two times when object ‘ta1’ is destructed. So object ‘ta1’ has two copies of all members of ‘Person’, this causes ambiguities. The solution to this problem is ‘virtual’ keyword. We make the classes ‘Faculty’ and ‘Student’ as virtual base classes to avoid two copies of ‘Person’ in ‘TA’ class. (Concept of *Virtual* explained later in the *Virtual Function & Runtime Polymorphism (Function Overriding)* section)
* Virtual Base Class-: Virtual base classes are used in virtual inheritance in a way of preventing multiple “instances” of a given class appearing in an inheritance hierarchy when using multiple inheritances.
*                          #include<iostream> 
                         using namespace std; 
                         class Person { 
                         public: 
	                         Person(int x) { cout << "Person::Person(int ) called" << endl; } 
	                         Person()	 { cout << "Person::Person() called" << endl; } 
                         }; 
                         class Faculty : virtual public Person { 
                         public: 
	                         Faculty(int x):Person(x) { 
	                         cout<<"Faculty::Faculty(int ) called"<< endl; 
	                         } 
                         }; 
                         class Student : virtual public Person { 
                         public: 
	                         Student(int x):Person(x) { 
		                         cout<<"Student::Student(int ) called"<< endl; 
	                         } 
                         }; 
                         class TA : public Faculty, public Student { 
                         public: 
	                         TA(int x):Student(x), Faculty(x) { 
		                         cout<<"TA::TA(int ) called"<< endl; 
	                         } 
                         }; 
                         int main() { 
	                         TA ta1(30); 
                         } 
---
	                 Person::Person() called
                         Faculty::Faculty(int ) called
                         Student::Student(int ) called
                         TA::TA(int ) called
	---
* Note-: One important thing to note in the above output is, the default constructor of ‘Person’ is called. When we use ‘virtual’ keyword, the default constructor of grandparent 
	 class is called by default even if the parent classes explicitly call parameterized constructor. 
* Calling the parameterized constructor of the grandparent class
                                        #include<iostream> 
                                        using namespace std; 
                                        class Person { 
                                        public: 
	                                        Person(int x) { cout << "Person::Person(int ) called" << endl; } 
	                                        Person()	 { cout << "Person::Person() called" << endl; } 
                                        }; 
                                        class Faculty : virtual public Person { 
                                        public: 
	                                        Faculty() { 
	                                        cout<<"Faculty::Faculty(int ) called"<< endl; 
	                                        } 
                                        }; 
                                        class Student : virtual public Person { 
                                        public: 
	                                        Student() { 
		                                        cout<<"Student::Student(int ) called"<< endl; 
	                                        } 
                                        }; 
                                        class TA : public Faculty, public Student { 
                                        public: 
	                                        TA(int x):Student(), Faculty(), Person(x) { 
		                                        cout<<"TA::TA(int ) called"<< endl; 
	                                        } 
                                        }; 
                                        int main() { 
	                                        TA ta1(30); 
                                        } 
---   
                                        Person::Person(int ) called
                                        Faculty::Faculty(int ) called
                                        Student::Student(int ) called
                                        TA::TA(int ) called	
---
* Note-: In general, it is not allowed to call the grandparent’s constructor directly, it has to be called through parent class. It is allowed only when ‘virtual’ keyword is used. 
* e.g. 2 -> If B and C are sub classes of A and are superclasses of D then to access the data members of the grandparent class we can use :: 
*                                       obj.ClassB::a = 10; 
	                                obj.ClassC::a = 100;
                                        obj.b = 20; 
	                                obj.c = 30; 
	                                obj.d = 40;						  
---
- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) - ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+)
## Exception Handling-:
* Terminate function-: Library function that is called if an exception is not caught. Terminate aborts the program. If no appropriate catch is found, execution is transferred to a library function named terminate. The behavior of that function is system dependent but is guaranteed to stop further execution of the program. Exceptions that occur in programs that do not define any try blocks are handled in the same manner: After all, if there are no try blocks, there can be no handlers. If a program has no try blocks and an exception occurs, then terminate is called and the program is exited.	
* The fact that control passes from
throw clause to a catch block, has two important implications:
 • Functions along the call chain may be prematurely exited.
 • When a handler is entered, objects created along the call chain will have been
destroyed.
* what() method-: Each of the library exception classes defines a member function named what. These functions take no arguments and return a C style character string (i.e., a const char*).
* Stack Unwinding-: The process of removing function entries from function call stack at run time is called Stack Unwinding. Workflow-: If the throw appears inside a try block, the catch clauses associated with that try are examined. If a matching catch is found, the exception is handled by that catch. Otherwise, if the try was itself nested inside another try, the search continues through the catch clauses of the enclosing trys. If no matching catch is found, the current function is exited, and the search continues in the calling function. If the call to the function that threw is in a try block, then the catch clauses associated with that try are examined. If a matching catch is found, the exception is handled. Otherwise, if that try was nested, the catch clauses of the enclosing trys are searched. If no catch is found, the calling function is also exited. The search continues in the function that called the just exited one, and so on.
* Note-: When a block is exited during stack unwinding, the compiler guarantees that objects created in that block are properly destroyed. If a local object is of class type, the destructor for that object is called automatically.
---
                                        #include <iostream> 
                                        using namespace std; 
                                        void f1() throw (int) { 
                                        cout<<"\n f1() Start "; 
                                        throw 100; 
                                        cout<<"\n f1() End "; 
                                        } 
                                        void f2() throw (int) { 
                                        cout<<"\n f2() Start "; 
                                        f1(); 
                                        cout<<"\n f2() End "; 
                                        } 
                                        void f3() { 
                                        cout<<"\n f3() Start "; 
                                        try { 
	                                        f2(); 
                                        } 
                                        catch(int i) { 
                                        cout<<"\n Caught Exception: "<<i; 
                                        } 
                                        cout<<"\n f3() End"; 
                                        } 
                                        int main() { 
                                        f3(); 
                                        getchar(); 
                                        return 0; 
                                        } 
			---
* Output-:                              f3() Start
                                         f2() Start
                                         f1() Start
                                         Caught Exception: 100
                                         f3() End
* Object destruction-: When an exception is thrown, destructors of the objects (whose scope ends with the try block) is automatically called before the catch block gets exectuted. Destructors are only called for the completely constructed objects. When constructor of an object throws an exception, destructor for that object is not called. Reason-:  In C++ the lifetime of an object is said to begin when the constructor runs to completion. And it ends right when the destructor is called. If the constructor throws, then the destructor is not called.
* 
---
                                        #include <iostream> 
                                        using namespace std; 
                                        class Test1 { 
                                        public: 
                                        Test1() { cout << "Constructing an Object of Test1" << endl; } 
                                        ~Test1() { cout << "Destructing an Object of Test1" << endl; } 
                                        }; 
                                        class Test2 { 
                                        public: 
                                        Test2() { cout << "Constructing an Object of Test2" << endl; 
			                                        throw 20; } 
                                        ~Test2() { cout << "Destructing an Object of Test2" << endl; } 
                                        }; 
                                        int main() { 
                                        try { 
	                                        Test1 t1; // Constructed and destructed 
	                                        Test2 t2; // Partially constructed 
	                                        Test1 t3; // t3 is not constructed as this statement never gets executed 
                                        } catch(int i) { 
	                                        cout << "Caught " << i << endl; 
                                        } 
                                        }	
						  ---
* Output-: Constructing an Object of Test1
  Constructing an Object of Test2
  Destructing an Object of Test1
  Caught 20
* User Defined Exception-:
---
				        #include <iostream> 
				        using namespace std; 
				        class demo1 {
						  demo1(){
						  cout<<"demo1 constructor called"<<endl;
				        }; 
				        class demo2 { 
						  demo2(){
						  cout<<"demo2 constructor called"<<endl;
				        }; 
				        int main() 
				        { 
					        for (int i = 1; i <= 2; i++) { 
						        try { 
							        if (i == 1) 
								        throw demo1(); 
							        else if (i == 2) 
								        throw demo2(); 
						        } 
						        catch (demo1 d1) { 
							        cout << "Caught exception of demo1 class \n"; 
						        } 
						        catch (demo2 d2) { 
							        cout << "Caught exception of demo2 class \n"; 
						        } 
					        } 
				        } 
---
* Output-: 				          demo1 constructor called
						  Caught exception of demo1 class
						  demo2 constructor called
						  Caught exception of demo2 class 
* Rethrowing an exception-: Sometimes a single catch cannot completely handle an exception. After some corrective actions, a catch may decide that the exception must be handled by a
function further up the call chain. A catch passes its exception out to another catch by rethrowing the exception. A rethrow is a throw that is not followed by an expression:
throw;
An empty throw can appear only in a catch or in a function called (directly or indirectly) from a catch. If an empty throw is encountered when a handler is not active, terminate is called. A rethrow does not specify an expression; the (current) exception object is passed up the chain. In general, a catch might change the contents of its parameter. If, after changing its parameter, the catch rethrows the exception, then those changes will be propagated only if the catch’s exception declaration is a reference:
* 				        catch (my_error &eObj) { // specifier is a reference type
				        eObj.status = errCodes::severeErr; // modifies the exception object
				        throw; // the status member of the exception object is severeErr
				        } 
				        catch (other_error eObj) { // specifier is a nonreference type
				        eObj.status = errCodes::badErr; // modifies the local copy only
				        throw; // the status member of the exception object is unchanged
				        }
						  ---
* Finally block implementation in c++ -: C++ doesn't support the finally block (which is used in programming languages like Java) instead it uses a concept, RAII (Resource Acquisition Is Initialization). The idea is that an object's destructor is responsible for freeing resources. When the object has automatic storage duration, the object's destructor will be called when the block in which it was created exits -- even when that block is exited in the presence of an exception. Actual code and implementation is out of the scope of this repo and most of the c++ books. 
