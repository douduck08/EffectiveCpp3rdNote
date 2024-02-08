# 《Effective C++ 3rd Edition》筆記

《Effective C++ Third Edition - 55 Specific Ways to Improve Your Programs and Designs》作者為 Scott Meyers。此書主要講述進行C++開發時，應該要注意與遵守的要點，可以避免程式出現難以察覺的錯誤、減少維護難度。

閱讀時間 2022/01/24 ~ 2022/02/10

## 目錄
### Introduction
[Note](./notes/intro.md)  

### Chapter 1. Accustoming Yourself to C++  
[Note](./notes/ch1.md)  
Item 1: View C++ as a federation of languages  
Item 2: Prefer consts, enums, and inlines to #defines  
Item 3: Use const whenever possible  
Item 4: Make sure that objects are initialized before they're used  

### Chapter 2. Constructors, Destructors, and Assignment Operators  
[Note](./notes/ch2.md)  
Item 5: Know what functions C++ silentlywrites and calls  
Item 6: Explicitly disallow the use of compiler-generated functions you do not want  
Item 7: Declare destructors virtual in polymorphic base classes  
Item 8: Prevent exceptions from leaving destructors  
Item 9: Never call virtual functions during construction or destruction  
Item 10: Have assignment operators return a reference to *this  
Item 11: Handle assignment to self in operator=  
Item 12: Copy all parts of an object  

### Chapter 3. Resource Management  
[Note](./notes/ch3.md)  
Item 13: Use objects to manage resources.  
Item 14: Think carefully about copying behavior in resource-managing classes.  
Item 15: Provide access to raw resources in resource-managing classes.  
Item 16: Use the same form in corresponding uses of new and delete.  
Item 17: Store newed objects in smart pointers in standalone statements.  

### Chapter 4. Designs and Declarations  
[Note](./notes/ch4.md)  
Item 18: Make interfaces easy to use correctly and hard to use incorrectly  
Item 19: Treat class design as type design  
Item 20: Prefer pass-by-reference-to-const to pass-by-value  
Item 21: Don't try to return a reference when you must return an object  
Item 22: Declare data members private  
Item 23: Prefer non-member non-friend functions to member functions  
Item 24: Declare non-member functions when type conversions should apply to all parameters  
Item 25: Consider support for a non-throwing swap  

### Chapter 5. Implementations  
[Note](./notes/ch5.md)  
Item 26: Postpone variable definitions as long as possible.  
Item 27: Minimize casting.  
Item 28: Avoid returning "handles" to object internals.  
Item 29: Strive for exception-safe code.  
Item 30: Understand the ins and outs of inlining.  
Item 31: Minimize compilation dependencies between files.  

### Chapter 6. Inheritance and Object-Oriented Design  
[Note](./notes/ch6.md)  
Item 32: Make sure public inheritance models "is-a."  
Item 33: Avoid hiding inherited names  
Item 34: Differentiate between inheritance of interface and inheritance of implementation  
Item 35: Consider alternatives to virtual functions  
Item 36: Never redefine an inherited non-virtual function  
Item 37: Never redefine a function's inherited default parameter value  
Item 38: Model "has-a" or "is-implemented-in-terms-of" through composition  
Item 39: Use private inheritance judiciously  
Item 40: Use multiple inheritance judiciously  

### Chapter 7. Templates and Generic Programming  
[Note](./notes/ch7.md)  
Item 41: Understand implicit interfaces and compile-time polymorphism  
Item 42: Understand the two meanings of typename  
Item 43: Know how to access names in templatized base classes  
Item 44: Factor parameter-independent code out of templates  
Item 45: Use member function templates to accept "all compatible types."  
Item 46: Define non-member functions inside templates when type conversions are desired  
Item 47: Use traits classes for information about types  
Item 48: Be aware of template metaprogramming  

### Chapter 8. Customizing new and delete  
[Note](./notes/ch8.md)  
Item 49: Understand the behavior of the new-handler  
Item 50: Understand when it makes sense to replace new and delete  
Item 51: Adhere to convention when writing new and delete  
Item 52: Write placement delete if you write placement new  

### Chapter 9. Miscellany  
[Note](./notes/ch9.md)  
Item 53: Pay attention to compiler warnings  
Item 54: Familiarize yourself with the standard library, including TR1  
Item 55: Familiarize yourself with Boost.  