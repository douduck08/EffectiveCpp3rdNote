# Chapter 2. Constructors, Destructors, and Assignment Operators
## Item 5: Know what functions C++ silentlywrites and calls
即使沒有宣告，只要有需要，編譯器也會自動為class建立幾個成員
* default constructor
* destructor
* copy constructor
* copy assignment operator

如有下情況，編譯器不會自動生成`copy assignment operator`
* 有reference成員
* 有const成員
* 其他直接copy或賦值並不合法的情況

## Item 6: Explicitly disallow the use of compiler-generated functions you do not want
不希望類別支援copy constructor或copy assignment operator時
* 宣告private版本，讓編譯器不自動生成

> 補充：
> C++11可以透過關鍵字delete/default明確控制自動生成的方法

## Item 7: Declare destructors virtual in polymorphic base classes
* 如果class中包含了virtual function，則必須宣告一個virtual destructor
* 如果class會被繼承，為了避免pointer沒正確釋放記憶體，則必須宣告一個virtual destructor
* 如果class沒有計畫被用來繼承，不需要宣告virtual destructor

## Item 8: Prevent exceptions from leaving destructors
* 在destructor直接丟出exception，可能會導致STL容器或其他一次大量釋放物件資源的工作被中斷，造成memory leak。
* 在destructor如果必須呼叫可能丟出exception的方法，使用catch

## Item 9: Never call virtual functions during construction or destruction
* C++中有個叫VPTR跟VTABLE的機制，有virtual function的class會有VPTR去指向它專屬的VTABLE。
* 如果derived class重新定義了一個virtual function，則VTABLE中紀錄的方法位址會指向新的方法，否則會指向base class的方法。

本Item重點
* 當一個derived class正要被建構時，它會先呼叫base class的建構子，然後建立base class的VTABLE，最後才是derived class的VTABLE。
* 所以如果在建構子中呼叫virtual function，會呼叫到預期之外的版本(base class)，設計邏輯上容易失誤，所以應該避免。
* 解構子同理。

不只virtual functions
* runtime type information也是在建構derived class時，還在處理中的資訊，所以不應該在建構子中使用`dynamic_cast`、`typeid`。

有些complier可以對這狀況提出警告，但如果是層層呼叫的情況下使用到virtual function，不一定會被抓出來，需要自己注意。

> 補充：
> C++ 解析 - 虛擬函數 Virtual Function
> http://ublearning.blogspot.com/p/virtual-function.html

## Item 10: Have assignment operators return a reference to `*this`
任何賦值運算子，都應該回傳`*this`

## Item 11: Handle assignment to self in operator=
實作賦值運算子時，要注意
* self-assignment-safe: 如果賦值自己，會不會誤把自己給delete?
* exception-safe: 如果賦值中途因為exception中斷，會不會有memory leak?

任何function都要考慮，傳入的多個變數中如果有相同物件，是否能正常運作。

## Item 12: Copy all parts of an object
一個良好的設計，只有兩個方法會複製物件，這兩個方法也會由編譯器自行生成
* copy constructor
* copy assignment operator

如果有自己宣告copy constructor，
* 每次新增member data時，都要記得回頭更新copy constructor，還有其他constructor。
* 如果有繼承，base class也要記得處理copy動作，放到initialization list中。
* 沒有被手動複製的object資料，編譯器會採用預設建構子去初始化。
* 不要在copy assignment operator直接呼叫copy constructor，反過來也不行。