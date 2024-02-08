# Chapter 4. Designs and Declarations
## Item 18: Make interfaces easy to use correctly and hard to use incorrectly
C++的一切都建立在interface上，一些容易發生的人為錯誤也能透過interface的設計去阻止。
* 以類型提示function傳入值
* function以回傳shared_ptr，取代pointer
    * 避免使用者忘記釋放資源
* shared_ptr可以自訂不同的deleter

## Item 19: Treat class design as type design
設計class時要考慮的問題
* 如何建立/銷毀物件，記憶體的配置/釋放
* 如何初始化，與賦值的差異在哪裡
* 如何pass by value (copy constructor的設計)
* 如何限制data member在允許範圍內，如何揪出資料錯誤 (member functions的設計)
* 如果新class繼承了其他class，或者允許被繼承，該注意甚麼 (Item 34.36.7)
* 要允許怎樣的轉型存在，顯性/隱性，單向/雙向 (Item 15)
* 需要哪些operator (Item 23.24.46)
* 要關閉那些standard functions (Item 6)
* 如何規劃data member的public/protected/private，是否需要friend class/function
* 定義undeclared interface (Item 29)
* 或許你需要的是template，而非class
* 你真的需要一個新class嗎?

## Item 20: Prefer pass-by-reference-to-const to pass-by-value
* 盡量使用pass-by-reference-to-const來傳遞class到function中。
* 對編譯器來說pass-by-reference-to-const會用類似pass-by-pointer的方式去處理。
* 少數適合pass-by-value的類型
    * built-in types
    * STL的iterators
    * function object types

## Item 21: Don't try to return a reference when you must return an object
* 不要將function中的local variable用reference形式回傳，生命週期會出問題。
* 直接回傳value，讓編譯器替你優化即可；如果不希望回傳值被當左值用(如`operator*`)，回傳const value。

## Item 22: Declare data members private
* 一律以function作為public介面，可以有更完整的read/write權限控制。
* 基本上不需要宣告public/protected成員變數。

## Item 23: Prefer non-member non-friend functions to member functions
* 如果不需要存取private成員(data/function)，就不需要把新功能設計成member function。
* 頃向將function寫為namespace之下的non-member function，或者某個utility class的member function。
* convenience functions可以寫在多個不同header file中。

## Item 24: Declare non-member functions when type conversions should apply to all parameters
* 不建議讓class支援implicit type conversions (non-explicit constructor)。
    * 例外：數字類別的轉型。
* 如果`operator*`宣告為member function，則implicit type conversion不能對lhs參數生效。
* 如果`operator*`宣告為non-member function，則implicit type conversion可對所有參數生效。

## Item 25: Consider support for a non-throwing swap
* 只要你的類別支援copy，則內建的`swap()`運作就會很順利，但這過程會用到三次copy。
* 為了優化特定類別的`swap()`，可以
    * 準備一個member swap function，它能存取類別的private member，確保它不會丟出exceptions
    * overloading function template: 在自己的namespace下寫一個non-member swap，呼叫member swap function
    * 如果不是class template，也準備一個std::swap
* 如果這個特定類別是class template，情況會再複雜一點，就不準備一個std::swap了
* 如果希望優先使用特化過的swap
    * 加上`using std::swap`
    * 呼叫時直接寫`swap()`
