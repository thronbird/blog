---
title:Kotlin官方文档摘录
---



## Idioms

```java
//Traversing a map/list of pairs
for ((k, v) in map) {
    println("$k -> $v")
}

//Lazy property
val p: String by lazy {
    // compute the string
}

//Extension Functions
fun String.spaceToCamelCase() { ... }
"Convert this to camelcase".spaceToCamelCase()

//Creating a singleton
object Resource {
    val name = "Name"
}

//asign
return if (x) foo() else bar()
return when(x) {
    0 -> "zero"
    else -> "nonzero"
}
val isEmpty: Boolean get() = size == 0

//Consuming a nullable Boolean
val b: Boolean? = ...
if (b == true) {
    ...
} else {
    // `b` is false or null
}

//Swapping two variables
var a = 1
var b = 2
a = b.also { b = a }

//Builder-style usage of methods that return Unit
fun arrayOfMinusOnes(size: Int): IntArray {
    return IntArray(size).apply { fill(-1) }
}

//Java 7's try with resources
val stream = Files.newInputStream(Paths.get("/some/file.txt"))
stream.buffered().reader().use { reader ->
    println(reader.readText())
}

```

## Coding Conventions

```java
//Always use immutable collection interfaces (Collection, List, Set, Map) to declare collections which are not mutated. 

//Default parameter values
fun foo(a: String = "a") { ... }

//If you have a functional type or a type with type parameters which is used multiple times in a codebase, prefer defining a type alias for it:

typealias MouseClickHandler = (Any, MouseEvent) -> Unit
typealias PersonIndex = Map<String, Person>

//Returns in a lambda:Avoid using multiple labeled returns in a lambda. Consider restructuring the lambda so that it will have a single exit point. If that's not possible or not clear enough, consider converting the lambda into an anonymous function.Do not use a labeled return for the last statement in a lambda.

//Using extension functions:Use extension functions liberally. Every time you have a function that works primarily on an object, consider making it an extension function accepting that object as a receiver. To minimize API pollution, restrict the visibility of extension functions as much as it makes sense. As necessary, use local extension functions, member extension functions, or top-level extension functions with private visibility.

```

## Returns and Jumps
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return // non-local return directly to the caller of foo()
        print(it)
    }
    println("this point is unreachable")
}
///The return-expression returns from the nearest enclosing function, i.e. foo.If we need to return from a lambda expression, we have to label it and qualify the return:
===》
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach // local return to the caller of the lambda, i.e. the forEach loop
        print(it)
    }
    print(" done with implicit label")
}

/Alternatively, we can replace the lambda expression with an anonymous function. A return statement in an anonymous function will return from the anonymous function itself.

fun foo() {
    listOf(1, 2, 3, 4, 5).forEach(fun(value: Int) {
        if (value == 3) return  // local return to the caller of the anonymous fun, i.e. the forEach loop
        print(value)
    })
    print(" done with anonymous function")
}

```

## Classes

```java
//Constructor
class DontCreateMe private constructor () { ... }
// parameters of the primary constructor can be used in the initializer blocks. They can also be used in property initializers declared in the class body:
class Customer(name: String) {
    val customerKey = name.toUpperCase()
}

```

## Inheritance

```java
//If the derived class has a primary constructor, the base class can (and must) be initialized right there, using the parameters of the primary constructor.
class Derived(p: Int) : Base(p)
```

Note:In Kotlin, unlike Java or C#, classes do not have static methods. In most cases, it's recommended to simply use package-level functions instead.you can write it as a member of an [object declaration](https://kotlinlang.org/docs/reference/object-declarations.html)inside that class.declare a [companion object](https://kotlinlang.org/docs/reference/object-declarations.html#companion-objects) inside your class

## visibility modifiers 

There are four visibility modifiers in Kotlin: `private`, `protected`, `internal` and `public`. The default visibility, used if there is no explicit modifier, is `public`.

## extension functions

extension functions are dispatched **statically**,

扩展方法可以重载但是不能重写原类方法：If a class has a member function, and an extension function is defined which has the same receiver type, the same name is applicable to given arguments, the **member always wins**.However, it's perfectly OK for extension functions to overload member functions which have the same name but a different signature.

```java
//This is what allows you to call toString() in Kotlin without checking for null: the check happens inside the extension function.
fun Any?.toString(): String {
    if (this == null) return "null"
    // after the null check, 'this' is autocast to a non-null type, so the toString() below
    // resolves to the member function of the Any class
    return toString()
}
```
## Generics

**The Existential Transformation: Consumer in, Producer out!** :-)

```java
//java 缺陷：there are no consumer-methods to call. But Java does not know this, and still prohibits it
interface Source<T> {
  T nextT();
}
//we have to declare objects of type Source<? extends Object>, which is sort of meaningless, because we can call all the same methods on such a variable as before
void demo(Source<String> strs) {
  Source<Object> objects = strs; // !!! Not allowed in Java
  // ...
}
====>
//kotlin - declaration-site variance: 
interface Source<out T> {
    fun nextT(): T
}
fun demo(strs: Source<String>) {
    val objects: Source<Any> = strs // This is OK, since T is an out-parameter
    // ...
}

```

## Functions

```java
//原函数：
fun reformat(str: String,
             normalizeCase: Boolean = true,
             upperCaseFirstLetter: Boolean = true,
             divideByCamelHumps: Boolean = false,
             wordSeparator: Char = ' ') {
...
}
//java 
reformat(str, true, true, false, '_')
====>
//kotlin - With named arguments we can make the code much more readable:
=====》
reformat(str,
    normalizeCase = true,
    upperCaseFirstLetter = true,
    divideByCamelHumps = false,
    wordSeparator = '_'
)
====>
reformat(str, wordSeparator = '_')

```