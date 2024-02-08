# 濃縮筆記
```cpp
class SomeClass
{
public:
    SomeClass() = default;                  // 預設建構子(default constructor)
    ~SomeClass() = default;                 // 解構子(destructor)
    SomeClass(const SomeClass &) = default; // 複製建構子(copy constructor)
    SomeClass(SomeClass &&) = default;      // 轉移建構子(move constructor)
    SomeClass & operator=(const SampleClass &) = default; // 複製指派運算子(copy assignment operator)
    SomeClass & operator=(SampleClass &&) = default;      // 轉移指派運算子(move assignment operator)
    
    explicit SomeClass(int); // 單一傳入值的建構子標記為明確(防自動轉型)
    virtual ~SomeClass();    // 有virtual function時，解構子標記為virtual
    
    virtual void dummy_func();
}
```

* 使用const/enum/inline等來取代`#define`的使用，能使用const的情境就盡量使用。
* 預設方法(建構子/解構子/assignment方法/copy方法)
    * 不需要的時候，要使編譯器不會自動生成。
    * 不要在建構子/解構子中呼叫virtual function。
    * 建構子只有一個傳入值時，記得標記為explicit方法。
    * 解構子要完全避免exception的可能性。
    * 有virtual function時，就要有virtual destructor。
    * assignment方法一律回傳`*this`，並記得處理自己指定給自己的情況。
    * copy方法記得維護，尤其有增加成員變數時。
* 資源管理物件(RAII)
    * 使用管理物件來進行資源釋放，不要手動處理。
    * 管理物件的建立一律寫為獨立語句，不要直接建立在方法傳入值的位置。
    * 管理物件要定義copy的行為。
    * 管理物件提供explicit方法來存取實際資源。
* 設計
    * 成員變數一律設為private，然後由public方法來定義讀寫權限。
    * 方法盡量使用pass-by-reference-to-const來傳遞參數。
    * 方法絕對不能回傳reference變數，除了`*this`。
    * 優先設計non-member non-friend functions，必要時才設計member functions。
    * 考慮設計一個優化過的專屬`swap()`。
* 實作
    * 確保資料有被初始化，善用member initializer list。
    * 盡量推遲變數宣告，盡量避免轉型。
    * 實作exception-safe的方法。
    * 善用forward declaration、pointer，降低檔案間的依賴。
* 繼承
    * 避免hiding inherited name(變數跟方法都是)，如果發生了記得unhide。
    * 考慮virtual function、NVI idiom、strategy pattern之間的選擇。
* 模板
    * 要追蹤模板的副作用：執行檔的大小、編譯時間、屬於implicit interfaces。
    * traits class的應用。
* new/delete
    * `std::set_new_handler()`的應用。
    * 注意自訂new/delete時的各種細節。

## 補充資料
* C++ 解析 - 虛擬函數 Virtual Function
http://ublearning.blogspot.com/p/virtual-function.html
* C++ Traits Classes
https://www.cnblogs.com/zhxmdefj/p/12530982.html