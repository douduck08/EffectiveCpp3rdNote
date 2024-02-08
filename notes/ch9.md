# Chapter 9. Miscellany
## Item 53: Pay attention to compiler warnings
* C++編譯器的警告，最後有可能會變成錯誤，不能忽視
* 不要依賴編譯器的警告，不同的編譯器可能會有不同的警告

## Item 54: Familiarize yourself with the standard library, including TR1
* C++98標準包含了
    * The Standard Template Library (STL)：主要包含containers, iterators及相關演算法
    * Iostreams：主要包含cin與cout等buffering IO
    * Support for internationalization：支援16 bits Unicode字元的wchar_t, wstring
    * Support for numeric processing：支援complex number跟arrays of pure values
    * An exception hierarchy：包含exception，以及延伸的類別logic_error, runtime_error
    * C89's standard library：可以支援C++的C語言函式庫
* Technical Report 1(TR1)
    * 提供14個功能組件，都放在std::tr1這namespace之下
    * `TR1::shared_ptr`、`tr1::function`、`tr1::bind`
    * Hash tables、Regular expressions、Tuples
    * `tr1::array`，使用起來很像STL容器的array
    * `tr1::mem_fn`
    * `tr1::reference_wrapper`
    * Random number generation
    * Mathematical special functions
    * C99 compatibility extensions
    * Type traits
    * `tr1::result_of`
* TR1不屬於C++標準，但其中很大一部份內容是C++標準的新功能候補
* TR1有很大一部份是來自Boost，使用上兩者差異不大

> 補充：
> 後來C++11已經包含了TR1中的不少功能

## Item 55: Familiarize yourself with Boost.
> https://www.boost.org/
> Boost是個高品質、跨平台的open source c++ library