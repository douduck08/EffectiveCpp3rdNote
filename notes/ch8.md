# Chapter 8. Customizing new and delete
## Item 49: Understand the behavior of the new-handler
* 如果new運算子沒有成功分配記憶體，根據編譯器版本，可能會給一個exception，或者回傳null ptr。
* 可以透過`std::set_new_handler(handlerFunction);`，來設定當記憶體分配失敗時要呼叫的方法。
    * new-handler會持續重複被呼叫，直到記憶體分配成功。
* 為了避免程式卡在重複呼叫new-handler的狀況中，設定的handler function應該要
    * 想辦法製造出可用記憶體
    * 替換成其他handler function，或將之設定為null
    * 丟出exception
    * 呼叫abort或exit
* 無法針對類型有不同的handler function，但可以自己實作一個
    * 在每次呼叫new運算子之前設定handler function
    * 記憶體分配成功後回復到原本的handler function(利用Item 13)
    * 可以進一步把這流程設計成模板，當自訂類型需要時就進行繼承

## Item 50: Understand when it makes sense to replace new and delete
* 需要自定義new/delete運算子的原因
    * 偵測錯誤: 想辦法抓出memory leak
    * 收集使用資訊
    * 希望增加效率，先用進行profiler確定new/delete是瓶頸，並建議用現成函式庫輔助實作
    * 希望減少記憶體用量(減少overhead)
    * 如果編譯器沒有採用最佳的aligned，希望自行改善
    * 希望將關聯資料放在記憶體的相鄰位置，提高讀取效率
    * 希望做出非預設的行為，如delete記憶體時將資料全部歸零
* 有些硬體會需要用特定方式去配置記憶體來達到最大效率(如four-byte aligned、eight-byte aligned)
    * 透過malloc取得記憶體，通常就會符合aligned規則
    * 但如果呼叫malloc時自行增加offset，可能就會打破規則，造成crash
* 自訂記憶體管理要兼顧效率是一件困難的事，建議直接採用現成的函式庫(如Pool library from Boost)

## Item 51: Adhere to convention when writing new and delete
* 實作new運算子，需要
    * 正確地回傳結果
    * 記憶體不足時呼叫new-handling function
    * 處理要求零記憶體的情況
    * 避免hide內建的new運算子
* new運算子要回傳pointer，辦不到的話就丟出exception bad_alloc
    * 如果new-handling function是null，才會丟出exception
    * 就算被要求zero byte的記憶體，也要回傳合法的pointer(比如至少回傳1 byte)
* 如果想要處理array的配置，要自定義new[]運算子(array new)
* 實作delete運算子，需要
    * 避免delete nullptr
    * 避免釋放錯誤的記憶體大小
* 如果base class沒有virtual destructor，derived class可能會在delete的時候出錯
* new/delete運算子是會子類別被繼承的，如果類別預計會被繼承，要注意處理

## Item 52: Write placement delete if you write placement new
* 考慮語句`Widget *pw = new Widget;`
    * 如果建構子執行失敗，丟出exception，則new所配置的記憶體應該要回收。
    * C++在這種情況會自動呼叫對應的delete function。
* 當你使用的是placement new (overload版本，有額外的參數)
    * 必要時，C++會去尋找相同簽名的placement delete來呼叫。
    * 如果找不到適當的placement delete，C++就甚麼都不會做。
* 如果寫了一個placement new，卻沒有寫對應的lacement delete，就會在建構失敗時造成memory leak。
* 實作placement new/delete當做member function時，要注意name hide問題，要讓普通版本的new/delete維持在可見的狀態。
