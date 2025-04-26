# Basic questions
## What is OOP?
OOP is a way of building programs using objects.  
- Each object is an instance of a class.
- Classes are organized in hierarchies through inheritance.
- Objects interact by sending messages to each other.
- Objects have their own state, which can only be changed by receiving a message.
  
A program is truly object-oriented **only if** it uses objects, classes, and inheritance together.<br>
[<sub><sup>source</sup></sub>](https://github.com/enhorse/java-interview/blob/master/oop.md#%D1%87%D1%82%D0%BE-%D1%82%D0%B0%D0%BA%D0%BE%D0%B5-%D0%BE%D0%BE%D0%BF)
# Design patterns
# Java questions
# Java.Collections questions
# Kotlin questions
# Kotlin.Coroutines questions

1. What are coroutines in Kotlin?

Coroutines are Kotlin’s native solution for writing asynchronous, non-blocking code. They allow developers to write sequential code that looks like traditional synchronous code while taking advantage of suspending functions and structured concurrency.



2. What is the difference between coroutines and threads?

Threads are managed by the operating system and are relatively expensive in terms of memory and context switching. Coroutines, on the other hand, are lightweight and can be multiplexed on a smaller number of threads. They provide a way to handle concurrency without the need for explicit thread management.


## How to create a coroutine in Kotlin?
Coroutines in Kotlin are created using coroutine builders like `launch`, `async`, or `runBlocking`.
- The `launch` builder is used for **fire-and-forget** coroutines that do not return a result. It returns a `Job` object representing the coroutine, which you can use to control or track its execution. You can wait for its completion using `Job.join()`, but it doesn't produce a result.
- The `async` builder is used when you need to return a result from the coroutine. It starts a new coroutine and returns a `Deferred` object, which represents a **Future** or **Promise**. The `Deferred` object stores the computation result, but the result is deferred and can be accessed by calling `await()`. This suspends the calling coroutine until the result is ready. You can also use `Deferred` with a specific type, such as `Deferred<Int>` or `Deferred<CustomType>`, depending on the lambda's return type.
- The `runBlocking` builder bridges regular and suspending functions, often used in `main()` functions or tests. It starts the top-level coroutine and blocks the thread until the coroutine finishes. While it is a blocking function, it is commonly used to test suspending code or launch coroutines in non-suspending contexts.

To start a coroutine, you need a **coroutine scope** for `launch` and `async`. `runBlocking` does not require a coroutine scope as it blocks the current thread until the coroutine finishes.

```kotlin
fun main() = runBlocking {
    val deferred: Deferred<Int> = async {
        loadData()
    }

    launch {
        // Fire-and-forget task (no output)
    }

    deferred.await()
}

suspend fun loadData(): Int {
    delay(1000L)
    return 42
}
```

## What is a coroutine scope?
To run any coroutine, you need a scope. Scope acts as a parent for all coroutines launched within it. If you cancel the scope, all its child coroutines are also cancelled.<br>
### Why do we need a Scope? 
When you launch a coroutine, it keeps running even if the result is no longer needed (a user closed the screen). You can get a `Job` from the builder like this:
```kotlin
val job = launch {
}
job.cancel()
```
However, if you launch many coroutines, managing their jobs individually becomes difficult. Instead, you can use a scope to manage all coroutines at once:
```kotlin
scope.launch {
}
scope.async {
}
scope.cancel() // Cancels all child coroutines
```
### What is CoroutineScope interface?
If you go to the source code of coroutines, you'll see that the `launch/async` function is actually an extension function of `CoroutineScope`:

```kotlin
fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext, 
    start: CoroutineStart = CoroutineStart.DEFAULT, 
    block: suspend CoroutineScope.() -> Unit
): Job
```
`CoroutineScope` is an interface that contains a `coroutineContext` property:

```kotlin
interface CoroutineScope {
    val coroutineContext: CoroutineContext
}
```
The `coroutineContext` is like a map that holds various elements (e.g., `Job`, `Dispatcher`, etc.).
By design, the context of a scope should always contain a `Job`.  
This is needed to support **structured concurrency** — child coroutines are tied to a parent `Job`, and cancelling the parent will automatically cancel all its children.
# Android questions
# Testing questions
# SQL questions
# GIT questions
# CI/CD questions
# KMP questions
