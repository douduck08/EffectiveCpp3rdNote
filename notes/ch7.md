# Chapter 7. Templates and Generic Programming
## Item 41: Understand implicit interfaces and compile-time polymorphism
* virtual function屬於runtime polymorphism
* template屬於compile-time polymorphism以及implicit interfaces
    * implicit interfaces: 對於型態T，template提供的是額外的interface
    * compile-time polymorphism: 對於不同型態的T，template都會呼叫它需要的方法(同名但實作可能不同)，屬於多型的表現
* 編譯器會自動檢查代入型態後，template能否執行

## Item 42: Understand the two meanings of typename
* C++編譯器預設模板中的nested dependent name不是一個type name，而是變數名。所以不能宣告由generic type(typename定義的那個)延伸出來的nested type
* 模板中必須要使用nested type，則在使用的位置之前加上`nested type`
    * 如`typename C::const_iterator iter(container.begin());`
    * 例外，在member initialization list則不需要加上`nested type`
    * 可以跟`typedef`連用，增加可讀性

## Item 43: Know how to access names in templatized base classes
* 當你用模板類別繼承模板類別，編譯器會無法確認templatized base class中有那些function可以呼叫
* 解決方式
    * 使用`this->`來明確呼叫者
    * 使用`using`讓方法可見
    * 使用class name來明確方法的歸屬

## Item 44: Factor parameter-independent code out of templates
* 使用commonality and variability analysis來確定模板的相關成本
* 使用模板的副作用
    * 編譯結果的二元檔容易大幅增大，模板中不依賴於template parameter的方法通常會形成浪費
    * 它是implicit的
* 盡可能減少使用模板產生出來的type/function
    * 有時將一個模板拆成多個模板，可以共用被實體化的(複雜的)function數量
    * 有時編譯結果的二元檔大小與編譯時間是無法兼顧的

## Item 45: Use member function templates to accept "all compatible types."
* 本來是繼承關係的類型，放入template parameter後產生的新類型就會失去繼承關係。
* generalized copy constructor: 利用member function templates來增加轉型的支援。
    * shared_ptr中便有這樣的設計
    * 這不影響預設的copy constructor生成，但如果要明確控制，需要同時自己宣告non-template copy function

## Item 46: Define non-member functions inside templates when type conversions are desired
* 在Item 24提到，如果在class中定義`operator*`，會因為lhs不接受轉型而降低通用性，建議改成non-member function。
* 若在class template中定義`operator*`，編譯器會因為找不到適當的轉型方式去對應實例化function，所以lhs、rhs都不接受轉型。
    * class tempele的member function不會去嘗試使用implicit type conversions
    * 加上friend function，定義non-member function template，便可以使用implicit type conversions
    * 為了讓編譯器成功link，需要提前宣告class template跟non-member function template

## Item 47: Use traits classes for information about types
* STL的五種iterator
    * Input iterators: 只能往前移動，每次移動1單位，只能read。通常用來讀取檔案，如istream_iterator
    * Output iterators: 只能往前移動，每次移動1單位，只能write。通常用來寫入檔案，如ostream_iterator
    * Forward iterators: Input iterators與Output iterators的綜合。
    * Bidirectional iterators: 在Forward iterators之上增加向後移動的功能。如Iterators for the STL list
    * Random access iterators: 可以往前或往後，並在固定時間內移動任何距離。如Iterators for vector, deque, and string
* traits classes
    * 利用Template Specialization來取得類型中的附加資訊
    * 利用Patial Spcialization來讓取得資訊的作法有所分別(比如對pointer做特化)
    * 當作資訊的類型通常為empty struct，可以彼此繼承
* 比對trait資訊時，可以
    * 用typeid來比較類型是否相同
    * 配合overload與traits classes，可以做到compile-time的if-else邏輯去選擇運作行為。

> 自行補充：
> C++ Traits Classes
> https://www.cnblogs.com/zhxmdefj/p/12530982.html

## Item 48: Be aware of template metaprogramming
* template metaprogram: 運作於C++編譯器中，根據模板產生sourde，然後編譯
    * 可以簡化本來runtime中做起來很複雜或不可行的工作
    * 把運作從runtime轉移到compile-time，讓錯誤被提早發現，runtime的運作效率也會提高
    * 缺點是編譯時間會拉長、不好debug
* 比如，在Item 47中把typeid比對的做法改用overload
* TMP做不到loop，但可以做到recursive template instantiations
