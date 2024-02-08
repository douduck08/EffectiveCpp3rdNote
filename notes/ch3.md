# Chapter 3. Resource Management
## Item 13: Use objects to manage resources.
資源管理的兩個要點
* 資源被取用後要立刻放入資源管理物件中。Resource Acquisition Is Initialization (RAII)。
* 資源管理物件透過destructor來確保釋放資源。

使用物件的destructor來釋放記憶體，避免任何會使delete沒被執行到的機會。如
* auto_ptr，使用時要注意，不要讓兩個以上auto_ptr指向同一個物件。
* [C++11] unique_ptr / shared_ptr，屬於 reference-counting smart pointer (RCSP)。
* auto_ptr / unique_ptr / shared_ptr 內部都是使用`deleate`，而非`deleate []`，不應用在陣列上。

## Item 14: Think carefully about copying behavior in resource-managing classes.
自己設計RAII物件時，要規劃好copy動作要如何處理，比如
* 禁止copy，大部分情況是用這個邏輯
* Reference-count，如shared_ptr背後的邏輯
* Copy the underlying resource，如std::string背後的原理
* Transfer ownership，如auto_ptr背後的邏輯

## Item 15: Provide access to raw resources in resource-managing classes.
RAII class需要提供一些explicit conversion或implicit conversion，可以存取內部資源。
* 如，shared_ptr提供了explicit conversion get()來取得raw pointer。
* 如，shared_ptr overload一般指標的運算子`operator->`、`operator*`。
* 通常只提供explicit conversion是比較常見安全的作法，根據特定需要才會有implicit conversion。

## Item 16: Use the same form in corresponding uses of new and delete.
`new`執行時
* memory is allocated
* 呼叫一個或多個constructor

`delete`執行時
* 呼叫一個或多個destructor
* memory is deallocated
* 釋放的記憶體大小，根據呼叫的destructor種類跟數量來決定，對pointer執行時，他會當作只有"一個"物件

結論
* 使用`new`搭配`delete`
* 使用`new ...[]`搭配`delete []`

## Item 17: Store newed objects in smart pointers in standalone statements.
* RAII物件使用時，不應該直接將RAII物件建構在function的傳入參數中，因為傳入參數的建構子執行順序是不明確的。
* 永遠都將RAII物件建構寫為一個獨立語句，如先儲存為變數，再進行其他動作。