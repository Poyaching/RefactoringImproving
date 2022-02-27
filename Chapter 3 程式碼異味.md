# Chapter 3 程式碼異味

###### tags: `重構改善既有程式的設計` `閱讀筆記`


### Mysterious Name(神秘的名稱)


重新命名也是重構的方法，好的命名可以讓人上天堂，壞的命名可以讓人困再爬程式碼地獄

尤其是在撰寫方法時，命名必須要貼切方法做的目的

(同時方法也必須要以只做一件事為準則，不要偷偷塞其他事情進去)

### Duplicated Code(重複的程式碼)

三種方法：

extract function：都是同一種用途

slide Statements：相似但不完全一致

pull up method：同一基礎類別的不同子類別裡面

### LongFunction(冗長的函式) & Long Parameter List(冗長的參數列)

擁有解釋性、共用性、選擇性的好處

過去語言呼叫函式有其他的銷耗，但現在語言已經移除銷耗，只要在IDE合適下，可以多用小函式，但同時也讓命名有重要性，函式的長度不是重點，而是做什麼

- **Decompose Conditional 分解條件式**

```jsx
// 原始程式碼
if(a && a.length > 0 ){
}else{
}
```

```jsx
// 分解條件式
if(isTrue(a)){
}else{
}
isTrue(a){
	return a && a.length > 0 
}
```

- **extract function 提取成方法**
    
    如果有需要特別寫註解說明時，就很適合提取成方法
    

```jsx
// 原始程式碼
name(a){
	const b = a+b
	// console.log
	console.log(a)
	console.log(b)
}
```

```jsx
name(a){
	const b = a+b
	consoleLog(a,b)
}
consoleLog(a,b){
	console.log(a)
	console.log(b)
}
```

- **Replace Temp with Query 轉成查詢變數**

```jsx
a=10
b=20
name() {
  const num = a * b;
  if (num > 1000) {
    return num * 0.95;
  }
    return null;
}
```

```jsx
a=10
b=20
name() {
  if (getNum()> 1000) {
    return num * 0.95;
  }
    return null;
}

getNum(){
	return a * b
}
```

- **Introduce Parameter Object 參數包裝成物件**
    
    在重構時，extract function很容易發生參數過多的情形，這時會將參數包裝成物件當作參數傳入
    
- **Preserve Whole Object 直接傳物件**
    
    如果說有時候，需要取某物件的幾個屬性，與其要分開傳入，不如直接傳入整個物件
    

```java
int empId = emp.getEmpId();
String empName = emp.getEmpName(); 
name(empId ,empName ){
// ....
}

```

```java
name(emp){
	int empId = emp.getEmpId();
	String empName = emp.getEmpName(); 
	// ....
}

```

### Global Data(全域資料)

作者號稱全域資料是第四層惡魔發明的，然後濫用的工程師就是在第四層受苦

全域變數很容易被濫用，在不知不覺的情況下改變，建議還是用封裝的方式，保護好全域變數，可以比較好的去控制跟掌控

### Mutable Data(可變資料)

除了全域變數資料，一般區域變數的資料也會面臨到被莫名更改的情況

這樣會讓錯誤不知不覺地發生

// TODO 找TSLint的ㄘㄨ

```jsx
let temp = 2 * (height + width);
console.log(temp);
temp = height * width;
console.log(temp);
```

```jsx
const perimeter = 2 * (height + width);
console.log(perimeter);
const area = height * width;
console.log(area);
```

### Divergent Change(發散式修改) vs Shotgun Surgery(霰彈式修改)

前者就是指**單一職責原則 Single Responsibility Principle，一個方法或class就是只做一件事，只要做其他事情，就是再拉成一個類別或方法**

書中舉例：一個系統將資料庫及業務邏輯放在同一個class中，只要有一個部分要調整時，必須要看完所有程式碼後，找到真正要調整的地方做更改，由於是寫在同一塊，所以很有可能相關程式碼是散落在各個角落，若將兩件事分開寫在不同class，那麼要調整時，只要更改其一，並了解其一的邏輯即可

而後者剛好相反，是相同邏輯的東西，分散在不同的class中，如果要調整，必須要所有使用到的都列出，並一一調整。

遇過問題：

### Feature Envy(依戀情結)

強調耦合度的重要性，在一段 A class 中，不斷的呼叫 B class 的方法，造成兩者間有高耦合

可以利用**[Move Method](https://refactoring.guru/move-method)和[Extract Method](https://refactoring.guru/extract-method)對症下藥**

- Move Mehtod ****
    
    ![Untitled](Copy%20of%20Chapter%203%20%E7%A8%8B%E5%BC%8F%E7%A2%BC%E7%95%B0%E5%91%B3%20397f98dde0824e6b97990556b56fd7e1/Untitled.png)
    
    將原本寫在內部的程式碼或方法抽出至另一個class，通常都會移到呼叫最多次的位置
    

但有設計模式中strategy and visitor 和 self delegation 剛好反其道而行

策略模式(Strategy)和 訪問者模式(Visitor)

委派模式(self delegation)

這兩個設計模式的主要目的都是避免Divergent Change，將「會一起改變的放在一起」

### Data Clunnmps(資料泥團)

資料庫抓出來的資料基本上都是成群結隊的，這寫資料就是適合存放在一個物件之中

甚至可以達到**Introduce Parameter Object 參數包裝成物件 和 Preserve Whole Object 直接傳物件的益處**

### Primitive Obsession(基本型態偏執)

有些本身就是一個單位，但偏執於用基本型別去宣告

譬如說時間、金錢，而這些最常被宣告的成字串

最常使用的解決方式：建立一個VO或者是enum

### Repeated Switches(重複的切換選輯)

### Loops(週圈)

stream遠比迴圈更加節省效能，甚至是易讀

### Lazy Element(冗員元素)

java本身沒有對於Lazy Element提供更有效的方式(最多只有Lambda擦點邊)，但在C++中或者Kotlin，為了避免這樣的情形會使用*inline function*

在Kotlin 中有一段程式碼

```kotlin
fun sum(a: Int, b: Int, lambda: (result: Int) -> Unit): Int {
    val r = a + b
    lambda.invoke(r)
    return r
}

fun main(args: Array<String>) {
    sum(1, 2) { println("Result is: $it") }
}
```

但它編譯完成後，等於(Function1)null.INSTANCE 只要在呼叫該方法時都會被自動產出，如果跑迴圈就會GG了

```kotlin
public static final void main(@NotNull String[] args) {
   //...
   sum(1, 2, (Function1)null.INSTANCE);
}

```

為了解決這樣的問題，Kotlin 提供了 inline 函式，上面的程式碼就會編譯成這樣，它不是再次產生一個(Function1)null.INSTANCE 

而是將fun sum()的方法，直接複製放進main方法中

```kotlin
public static final void main(@NotNull String[] args) {
	//...
	byte a$iv = 1;
	int b$iv = 2;
	int r$iv = a$iv + b$iv;
	String var9 = "Result is: " + r$iv;
	System.out.println(var9);
}
```

### Speculative Generality(畫大餅)

「可能需要用到」就多加了許多不需要的程式碼

譬如參數、不必要的interface

### Temporary Field(暫時欄位)

### Message Chains(過度藕合的訊息鏈)

### Middle Man(中間人)

### Insider Trading(內幕交易)