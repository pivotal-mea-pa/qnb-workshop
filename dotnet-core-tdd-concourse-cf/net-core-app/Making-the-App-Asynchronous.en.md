**Previous:** [[Introducing Fluent Assertions]]

As outlined briefly in [Asynchronous programming](https://docs.microsoft.com/en-us/dotnet/csharp/async), you should be writing asynchronous code when accessing a database. If operations may take a long time, you don't want to be locking up your threads.

When making a method `async`, you will need to change the return type to one of three things:  
`void` *Only used for event handlers*  
`Task` *For when you want to wait for the method to finish, but don't care about a return value*  
`Task<T>` *For when you want to wait for on the method call, and expect back an object of type `T`*

In this app, we will only be using the last one.

The key to calling asynchronous methods is to use the keyword `await`. This will cause it to wait until the method has completed and return the result from within the `Task<T>`.

As part of best practices, we'll also be renaming our asynchronous methods to have the suffix `Async` now.

**Note:** There are also times were you may not need to have your method be `async`, but still want to take advantage of the consistency of making your methods return a `Task<T>` (this is good for your interfaces). In such a situation, you can employ `Task.FromResult(x)`. What this means is that you can hand back a `Task<T>` that will return immediately when awaited.

**Git Tag:** [making-the-app-asynchronous](https://github.com/xtreme-steve-elliott/NotesApp/tree/making-the-app-asynchronous)

**Up Next:** [[Delegating Data Responsibility]]

**References**   
[Asynchronous Programming Advice](https://docs.microsoft.com/en-us/dotnet/csharp/async#important-info-and-advice)
