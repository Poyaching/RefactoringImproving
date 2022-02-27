# 【補充】Kotlin 的 inline function

###### tags: `重構改善既有程式的設計` 

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

如此就可以減少 lambda function 造成的新建物件 instance 的問題，減少記憶體的消耗

但要注意的是因為 inlining 會造成大量的程式碼，所以如果是很大的函數，不建議使用 inline 關鍵字。