# Chapter 6. Inheritance and Object-Oriented Design
## Item 32: Make sure public inheritance models "is-a."
* public inheritance會繼承所有東西
* "is-a"不是唯一的一個class間關係，也不是隨時都合邏輯的

## Item 33: Avoid hiding inherited names
* 使用相同名稱去overload base class的成員方法，會導致編譯器找不到本來base class中的成員方法
* 可以使用using讓base class的成員重新可見

## Item 34: Differentiate between inheritance of interface and inheritance of implementation
* pure virtual function
    * interface有pure virtual function，需要在子類別被實作
    * C++允許對pure virtual function提供實作，只是需要加上class name來呼叫
* 一般virtual function
    * 使用一般virtual function，可以有預設的implementation
    * 可能會忘記進行override
* non-virtual
    * 子類別無法變更內容，代表所有子類都會有的相同implementation
* 如果function的行為是多選一，則使用一般virtual function可能不是個好選擇
    * 可以採用pure virtual function，然後再提供其他protected function保存多種預設行為

## Item 35: Consider alternatives to virtual functions
* The Template Method Pattern
    * 又稱為non-virtual interface (NVI) idiom
    * 設計成private virtual member function，再透過public non-virtual member function呼叫
    * 可以有before跟after的固定行為
* The Strategy Pattern
    * 使用function pointers
    * 相同型態的instance可以有不同的行為
    * 可以允許動態變更
    * 缺點：如作為function pointer的是外部function，是沒辦法存取private成員的
* The Strategy Pattern
    * 使用tr1::function，兼具接受function pointer, function object, member function pointer的能力
    * 對傳入參數與回傳值的型態可以容許implicit convert
* The "Classic" Strategy Pattern
    * 使用另外一個class封裝行為

## Item 36: Never redefine an inherited non-virtual function
* non-virtual function的呼叫以變數的型態為主，如果存在同名的function，容易有設計失誤
* virtual function是動態綁定，呼叫以實體本身的型態為主 (Item 37))

## Item 37: Never redefine a function's inherited default parameter value
* static type: 變數宣告的型態。
* dynamic type: pointer指向的物件的型態。
* virtual function是dynamically bound, 所以是根據dynamic type來決定執行的function。
* 但是，default parameter是statically bound，所以會造成呼叫derived class的方法，卻採用了base class的預設值這種狀況。
* 所以，如果後續維護更改了base class function的預設值，則所有子類別都會受影響。
* 可以用NVI idiom解決這種設計需求。

## Item 38: Model "has-a" or "is-implemented-in-terms-of" through composition
* 區分繼承與組合的使用情境

## Item 39: Use private inheritance judiciously
* private inheritance
    * 子類別無法被當作父類別來運用，兩者不是is-a關係
    * 屬於is-implemented-in-terms-of關係
* 優先使用composition
* 只在必要時才用private inheritance，如：
    * 需要組合某類型，同時還要override其中的virtual function(依然有其他方式可以做到類似設計，雖然複雜一點)
    * 需要組合某類型，並存取其中的protected成員時
    * 想要組合一個zero-size class(裡面只有static成員)，又不想讓浪費記憶體時(作為成員的zero-size class，依然會增加class size)，這情形被稱為emptybase optimization (EBO)

## Item 40: Use multiple inheritance judiciously
* deadly MI diamond: 因為多重繼承，導致擁有兩份的成員變數
* virtual base class/virtual inheritance，C++為了解決上面問題的機制
    * 缺點：使用virtual inheritance的類別會變大，存取成員也會變慢
    * 除非必要，別用
    * 使用時盡量減少需要放在其中的member data
* 一般能用single inheritance(SI)解決的情境，就不需要特地動用MI；雖然不用刻意避免MI，但使用時要非常小心
    * 需要MI的情境：組合public inheritance與private inheritance
