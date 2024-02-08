# Introduction
## Terminology
* declaration
* signature: 定義中指function的傳入值組合
* definition
* initialization
    * default constructor `A();`
    * copy constructor `A(const A&);`
    * copy assignment operator `A& operator=(const A&);`
* undefined behavior
* function objects
* interface

## Naming Conventions
C++編譯器，將一些簡短的語法解釋為特定function呼叫，如：
* `operator*`、`operator==`的呼叫，自動傳入left-hand argument和right-hand argument變數
* 對member function來說，this指標相當於left-hand argument
