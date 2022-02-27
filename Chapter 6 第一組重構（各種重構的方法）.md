
# Chapter 6 第一組重構（各種重構的方法）

###### tags: `重構改善既有程式的設計` `閱讀筆記`

## Extract Function(提取函式)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-1.png?raw=true)

作者認為**將意圖與實作分開**為提取函式的原則（也有人認為超過一個IDE畫面或者使用超過一次） 
如果在看一段程式碼時，必須要花很大了心力才能理解大意，那就必須提取出來，並給他清楚的名字
這樣做整體理解的時候才可以快速地知道大致上的架構

基於這個原則，很容易產生非常小的函式，但這樣也代表，程式碼必須要一直呼叫函式，這樣會有對於呼叫程式的疑慮

「呼叫程式的成本和要執行的程式碼數量成本相符嗎？」(呼叫一次程式只有執行3~4行程式碼)

不過因為編譯器的優化，這些成本支出，和閱讀成本的支出反而不算什麼

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

## Inline Function(將函式内聯)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-2.png?raw=true)

和上面相反的，函式内聯是為了避免過多的函式，有時候函式的用途過於相近，反而會造成理解上的困擾

譬如A之中又包含了一個小函式B，但其實B有沒有抽離出來都不回讓程式碼理解變簡單(因為本來就很簡單了)

```jsx
// 原始程式碼
function getRating(driver) {
  return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver) {
  return driver.numberOfLateDeliveries > 5;
}
```

```jsx
function getRating(driver) {
  return (driver.numberOfLateDeliveries > 5) ? 2 : 1;
}
```

另外關於函式內聯補充資料：主要是談編譯器提供的函示內聯關鍵字

[Kotlin 的 *inline function*](https://www.notion.so/Kotlin-inline-function-15ca285745844fc294ffa6490aec08a2)

## Extract Variable(提取變數)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-3.png?raw=true)

簡單來說就是一直使用方法提取內容時，那不如將其用變數裝起來

## Inline Variable(內聯變數)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-4.png?raw=true)

## Change Function Declaration(修改函式宣告式)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-6.png?raw=true)

## Encapsulate Variable(封裝變數)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-7.png?raw=true)

## Rename Variable(更改變數名稱)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-8.png?raw=true)

## Introducer Object(使用參數物件)

## Combine Functions into Class(將函式移入類別)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-10.png?raw=true)

## Combine Functions into Transform(將函式組成轉換成函式)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-11.png?raw=true)

## Split Phase(拆成不同階段)

![Untitled](https://github.com/Poyaching/RefactoringImproving/blob/main/image/6-12.png?raw=true)