## What is dynamic in C#?
Dynamic is a new keyword introduced via the Dynamic Language Runtime(DLR) in C# 4. As we all know, C# was a statically typed language, which requires all the strong type check during the compiling phase.

To recall, programming language basically divided into two types: static and dynamic. Static language usually has strong type check and complied into the lower level language or machine instructions, while dynamic language usually are usually interpreted during the runtime by the interpreter (even they might be complited into bytecode for better performance like Python). Language like C, C++, Java, Go and C# are the static lanuage and Python, Javascript etc. are belonging to the dynamic language.  

The dynamic feature in C# brings more flexibility and capability, which makes the developer easily developer a lot of fansy feature/product, like IronPython, IronRuby etc. WHen using the dynamic keyword, you could invoke the function/method you think/know it has, and the C# runtime will do the actually method binding and invocation.

*Example code snippet:*
```
// During the compiling phase, compiler does not detect variable as the string type
dynamic dyn = "dynamic feature";

// Output 15
Console.WriteLine(dyn.Length);

// Throw the exception: [Microsoft.CSharp.RuntimeBinder.RuntimeBinderException: 'string' does not contain a definition for 'Count']
// Using the dynamic keyword will let the C# compiler not to do the typed check for dyn and there is no compiler error.
Console.WriteLine(dyn.Count);
```

According to the example above, the obivious difference when using dynamic is the the compiler does not do the type check for the dynamic variable and there is not compiler error for `dyn.Count`. If switching to using the concrete type `string` then it will throw the error during the compiling phase.

If you want to learn more about Dynamic Language Runtime, there are several further readings:
- [Dynamic Language Runtime Overview](https://docs.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/dynamic-language-runtime-overview)
- [Dynamic Programming in the .NET Framework](https://docs.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/)

## Why we need dynamic?
The greatest benefit of dynamic feature is bringing the capability of dynamic language to C#. Before having dynamic feature in C#, most of 'dynamic' features are implemented by reflection. However, there is not that straight forward to implement by reflection which makes the code looks not clean in logic perspective and sometimes hard to understand.

Another benefit is making the COM introp experience much better, then it does not need to use the casting from type `object` everywhere.

In terms for the comprasion of dynamic and reflection, please check out [Dynamic vs Reflection](#dynamic-vs-reflection).

## How dynamic (type) works internally?
> The type is a static type, but an object of type `dynamic` bypasses static type checking. In most cases, it functions like it has type `object`. At compile time, an element that is typed as `dynamic` is assumed to support any operation. 

Since the element of type `dynamic` is assumed to support any operation in the compiler, then it could bypass the compile-time type checking and do not occur any error during the compiling phase.

At most time, the `dynamic` acts like `object`. However, during the compiling phase, the compiler does interpret the real type of the `dynamic` element, but will store related meta for the `dynamic`. 

Take a closer look at the compiled IL code, you will find that the call of `dynamic` element is done by the objects of type `System.Runtime.CompilerServices.CallSite`, that is also why some articles mentioned that the `dynamic` is implemented via `object` in the underlying. So, the call will return to the method within the `CallSite` element and the call sites act as the entry point to determine the right target of dynamic operation invocation.

## When should we use dynamic?
* Use dynamic to simplify your private reflection code
* Use dynamic to avoid the casting of `object` element
* Use the dynamic in the COM Introp

What is more, you could check out the [Dynamic vs Reflection](#dynamic-vs-reflection) and see differences between them, then you have ideas about more on different scenarios.

## Dynamic performance
The compiler IL code for the call of `dynamic` element contains a lot of `CallSite` elements, which looks much heavier than the direct/static call, and even over than the reflection IL code. *How does it improve the performance for `dynamic` type?* The CLR will have cache for the `CallSite` elements.

There are some interesting [perf test experiments for the dynamic and reflection](http://geekswithblogs.net/SunnyCoder/archive/2009/06/26/c-4.0-dynamics-vs.-reflection.aspx).

## Dynamic vs Reflection
Even the dynamic looks similar to the reflection in some extent, and during the CallSite binding the dynamic might also rely on the reflection to but they are different two things. They are two tech used to implement the 'dynamic' behavior during the runtime.

Reflection are mostly used in the scenarios for controlling, like Unit test, Annations etc.

There is a detailed comparions from this [article](https://www.codeproject.com/Articles/593881/%2FArticles%2F593881%2FWhat-is-the-difference-between-2):

| 		|Reflection|Dynamic|
|-------|--------------|-------|
|Inspect (meta-data)|Yes|No|
|Invoke public members|Yes|Yes|
|Invoke private members|Yes|No|
|Caching|No|Yes|	
|Static class|Yes|No| 

## `Dynamic` vs `Object` vs `Var`
`dynamic` type in most cases act as `object` with supporting any operations. And it will not have error during the compiling phase but might occur exception if not found the operation for that element.

`object` is the base type for most of the types in C# (except pointer type, inteface, "open" type paramter types. More see [Not everything derives from object](https://blogs.msdn.microsoft.com/ericlippert/2009/08/06/not-everything-derives-from-object/)). 

However, using `object` will havce the disadvantages are:
- Need Casting code which make the code looks not clean
- Boxing and Unboxing, might have performance impact if in the hot path
    - Object allowed in heap, and if the concret element's type is struct which allowed in the stack, then it will need to do the boxing and unboxing.

`var` is equivalent to using the concret element type. It is just a syntax sugar. And there are some argument about whether using `var` or concret ele type (like string, int etc.) or not. My understanding is that as long as there is a consistant coding style, each one (or even mixed) should be ok.

`var` will have the best performance among these three. There are several advantages:
- Less code and better readability
    - Example: 
        - `var args = new DataGridViewCellContextMenuStripNeededEventArgs(...)` vs
        - `DataGridViewCellContextMenuStripNeededEventArgs args = new DataGridViewCellContextMenuStripNeededEventArgs(...)`
- Better for refactoring or type change. If you change to using a new type, you don't need to change the declaration in all places. 

There are some suggestons for `var`: [When to Use and Not Use var in C#](https://intellitect.com/when-to-use-and-not-use-var-in-c/).

**References:**
* [Understanding the Dynamic Keyword in C# 4](https://visualstudiomagazine.com/Articles/2011/02/01/Understanding-the-Dynamic-Keyword-in-C4.aspx)
* [Why is reflection slow?](https://mattwarren.org/2016/12/14/Why-is-Reflection-slow/) Matt Warren describes very detail about why the reflection slow and how to improve the performance
    * [List of .NET Performance books](https://mattwarren.org/resources/)
* [What is the difference between “dynamic” and “object” keywords?](https://blogs.msdn.microsoft.com/csharpfaq/2010/01/25/what-is-the-difference-between-dynamic-and-object-keywords/)
* [Using type dynamic (C# Programming Guide)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/using-type-dynamic)
* [dynamic (C# Reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/dynamic)
* [What is the difference between Reflection and dynamic keyword in C#?](https://www.codeproject.com/Articles/593881/%2FArticles%2F593881%2FWhat-is-the-difference-between-2)
* [C# 4.0 Dynamics vs. Reflection](http://geekswithblogs.net/SunnyCoder/archive/2009/06/26/c-4.0-dynamics-vs.-reflection.aspx) This article did some interesting performance test for Dynamics, Reflection and static/direct property call.
* [Not everything derives from object](https://blogs.msdn.microsoft.com/ericlippert/2009/08/06/not-everything-derives-from-object/)
* [Dynamic objects and Call Sites](https://www.wintellect.com/dynamic-objects-and-call-sites/)