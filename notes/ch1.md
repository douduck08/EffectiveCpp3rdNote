# Chapter 1. Accustoming Yourself to C++
## Item 1: View C++ as a federation of languages
將C++視為多個語言的集合
* C: C++語言的基礎
* Object-Oriented C++: 有關class的一切
* Template C++
* The STL

把以上四個群記在心中，方便理解一些程式技巧的效率。如在C語言的範圍內`pass-by-value`優於`pass-by-reference`，但加上Object-Oriented C++的功能後，`pass-by-reference-to-const`通常又成了最佳解。

## Item 2: Prefer consts, enums, and inlines to #defines
在效能考慮上，compiler階段計算優於runtime階段計算，但要避免使用preprocessor。preprocessor實際上不能當作程式語言的一部份，compiler沒有從中獲得資訊，它也無法被compiler優化。

使用constant遠優於define的兩個例子
* 字串常數，改用`const char * const`或者`const std::string`。
* 宣告為class member，限制變數的有效範圍。

另一個方案，enum hack。這方法可以避免"常數"被pointer指定，也常用在template的一些應用。
```cpp=
enum { NumTurns = 5 };
```

如果要代替define function macros，inline function template通常就能做到，而且更安全合理。

## Item 3: Use const whenever possible
```cpp=
char greeting[] = "Hello";
char *p = greeting; // non-const pointer, non-const data
const char *p = greeting; // non-const pointer, const data
char * const p = greeting; // const pointer, non-const data
const char * const p = greeting; // const pointer, const data
```
STL的iterator表現起來類似於T*：
* `const std::vector<int>::iterator iter`不能改變指向。
* `std::vector<int>::const_iterator cIter`不能用來更改內部值。

const的強大之處通常體現在function的宣告上：
* const回傳值提高安全性或效率，使回傳結果只能當右值使用。
* const傳入值本身沒有特別之處，但除非你需要在function中修改變數，不然一律使用const宣告。

const Member Functions：
* 用來清楚標示那些function不會修改物件data。
* 使這個function可以由const物件來呼叫使用。
* overload可以用於const function跟non-const function之間。
* 可在const function中修改mutable變數。

當overload const function跟non-const function，其中程式可能會duplication。這種情況可以透過`const_cast`、`static_cast`來處理。

## Item 4: Make sure that objects are initialized before they're used
在constructor內設值不算是初始化，使用member initialization list才是。
* 設值使用了copy assignment operator。
* 使用member initialization list時，會使用copy constructor，效率更好。
* member initialization list的順序不影響初始化順序，初始化順序只跟宣告的順序有關。所以永遠保持member initialization list的順序與宣告順序一致，避免誤會。

non-local static objects的初始化順序是不可控的，尤其是在橫跨不同source file的宣告上。
