# Chapter 5. Implementations
## Item 26: Postpone variable definitions as long as possible.
* 盡可能在使用到前再宣告變數
* 盡可能直接在建構時初始化變數，減少copy次數
* 根據constructor/destructors/assignments的成本，決定是否要在loop外就提早宣告變數

## Item 27: Minimize casting.
* C++中轉型比起C/Java/C#都危險，任何轉型都應該盡可能避免。
* old-style casts
    * (T)expression
    * T(expression)
* new-style or C++-style casts
    * const_cast\<T>(expression): 轉換const/non-const物件
    * dynamic_cast\<T>(expression): safe downcasting，但有額外消耗
    * reinterpret_cast\<T>(expression): low level的轉換，無視變數型態
    * static_cast\<T>(expression): 強制的implicit conversions，適用於最多情況
* old-style casts只用於呼叫explicit constructor，優先用new-style casts。
* 避免使用dynamic_cast，它對效能影響巨大。

## Item 28: Avoid returning "handles" to object internals.
* handles = reference, pointer, iterator
* 回傳private資料成員的handles，相當於讓成員的存取權限變為public
* 回傳內部資料成員的handles，可能會出現變數生命週期的錯誤
* 例外：`operator[]`回傳reference，因為要能夠被賦值修改參數

## Item 29: Strive for exception-safe code.
* exception-safe的方法，需要在exception發生時，確保
    * Leak no resources
    * Don't allow data structures to become corrupted
* exception-safe的方法，分成三種強度
    * the basic guarantee：假如發生exception，類型依然會維持在一個合理的狀態
    * the strong guarantee：假如發生exception，類型會恢復成呼叫function前的狀態
    * the nothrow guarantee
* 利用Item 13.14確保資源的管理
* 一個function的exception-safe強度，由它呼叫的function中強度最弱的那個決定

## Item 30: Understand the ins and outs of inlining.
* inline關鍵字可以被視為a request to compilers，需要將function的body完整寫在header file中。
* inline可能會變編譯器忽略
    * 內容過於複雜
    * 可以開啟編譯器的warning，來確認是否有成功
* function被function pointer指向的話，不會以inline方式呼叫
* inline function在修改後，所有呼叫它的相關程式碼都需要重新編譯
* debug模式通常無法處理inline function

## Item 31: Minimize compilation dependencies between files.
* forward declaration
    * 不要嘗試用在標準函式庫內的類型。
    * 為了明確類型的記憶體大小，每個成員data也都需要大小可知。
    * 使用pointer可以避開定義大小的需要。
* Handle class，pointer to implementation
    * 使用另外的class儲存成員資料 (implementation class)
    * 使用pointer組合
* Interface classe
    * 方法都是virtual function
