**Previous:** [Making the App Asynchronous](../making-the-app-asynchronous)

In the previous sections, we setup a bare-bones controller that simply returned a constructed list of notes from its endpoint. We want to get our app to the point where it can create and retrieve records. Soon a means to store said records will make sense, but for the moment, let's consider the responsibilities of a our controller.

We communicate with our controller through our REST endpoints (currently only parameterless `GET` is available). A list of `Note` is expected back. Our controller should not need to know exactly how our data is being stored or even where. All it should really know is how to communicate with something that can handle records for us. That something should encapsulate the knowledge of how to manage data. This sort of pattern is called the **Repository Pattern**. In this guide, we will also be employing a service to communicate with the repository and process any input/output as needed.

***

Let's extract methods for data access. Start by creating the interface for our repository. Make a new directory, `Repositories` and add a new file, `NoteRepository.cs` with the following interface:
```c#
public interface INoteRepository
{
    Task<IEnumerable<Note>> GetNotesAsync();
}
```
and initial implementation of said interface:
```c#
public class NoteRepository : INoteRepository
{
    public Task<IEnumerable<Note>> GetNotesAsync()
    {
        throw new NotImplementedException();
    }
}
```

The tests for the `NoteRepository` will look similar to the `NotesController` tests, as we are moving the source of the data down to this level.
```c#
[Fact]
public async Task GetNotesAsync_ShouldReturnNoteList()
{
    var expected = new List<Note>
    {
        new Note { Id = 1, Body = "Note 1" }
    };

    // _repository is assigned in the constructor of the test class
    var response = await _repository.GetNotesAsync();
    response.Should().NotBeNullOrEmpty().And.BeEquivalentTo(expected);
}
```

The implementation is fairly straight-forward:
```c#
public Task<IEnumerable<Note>> GetNotesAsync()
{
    return Task.FromResult(new[] { new Note { Id = 1, Body = "Note 1" } }.AsEnumerable());
}
```

***

**Note:** At this point, we could make use of the repository directly from the controller, but as the complexity of the app increases, it's best to abstract input and output manipulation of data (your business logic) into services. 

Create a new directory, `Services`, with the interface `INoteService`, defined as follows:
```c#
public interface INoteService
{
    Task<IEnumerable<Note>> GetNotesAsync();
}
```

Note that the interface is pretty much identical to that of `INoteRepository`. This is just because our app is very simple right now. In order to make use of `NoteRepository`, we'll need to inject `INoteRepository` into `NoteService`.

Create the `NoteService` class, and add a new constructor that accepts the injected `INoteRepository`:
```c#
private readonly INoteRepository _noteRepository;

public NotesService(INoteRepository noteRepository)
{
    _noteRepository = noteRepository;
}
```
We'll need to register `NoteRepository` in order for the injection to work.

Open up `Startup` and find `ConfigureServices(IServiceCollection services)`. Add `services.AddTransient<INoteRepository, NoteRepository>();` above the `services.AddMvc();` line. What this says is that when `INoteRepository` is required for injection, an instance of `NoteRepository` is created and provided. A new one is created each time an injection is needed (`AddTransient`).

***

Time for TDDing this service. Create new directory in `NotesApp.Tests` called `Services` to mirror `NotesApp`. Add a new class, `NoteServiceTests`. Since our service now requires an instance of `INoteRepository`, we need to provide it with one. However, we don't want to use a real one for our tests. We're going use mocks instead. The mocking framework that will be used for this is [Moq](https://github.com/moq/moq4). It's quite flexible, clean, and the setups are understandable. The most recent version as of this writing is `4.10.1`. You can add it as a dependency to `NotesApp.Tests` in much the way you added **FluentAssertions**.

Make a constructor for `NoteServiceTests` that instantiates the mock of `INoteRepository` and the service itself. Because of the way Moq works, the mock can be configured at any point before the method you care about is called on it.
```c#
private readonly Mock<INoteRepository> _noteRepositoryMock;
private readonly NoteService _service;

public NoteServiceTests()
{
    _noteRepositoryMock = new Mock<INoteRepository>();
    _service = new NoteService(_noteRepositoryMock.Object);
}
```

Add two tests to this class. The first test should check that calling the `GetNotesAsync` method in the service will call the `GetNotesAsync` method in the repository.
```c#
[Fact]
public async Task GetNotesAsync_ShouldCall_NoteRepository_GetNotesAsync()
{
    await _service.GetNotesAsync();
    _noteRepositoryMock.Verify(r => r.GetNotesAsync(), Times.Once);
}
```
Running this test will fail due to not being implemented, so for the moment, make the body of `GetNotesAsync` in `NoteService` just return `Task.FromResult<IEnumerable<Note>>(null)`.
 
Running it now will fail on `Verify` because we are not yet calling `GetNotesAsync`. To make the test pass, add a line to the `GetNotesAsync` above the `return` line:
```c#
_noteRepository.GetNotesAsync();
```

Now the test should pass.

The second test will ensure that when calling `GetNotesAsync` in our mock, the service returns the response from that call.
```c#
[Fact]
public async Task GetNotesAsync_ShouldReturn_NoteRepository_GetNotesAsync()
{
    // Create initial and expected (initial is the same as expected)
    // ...
    
    _noteRepositoryMock
        .Setup(r => r.GetNotesAsync())
        .ReturnsAsync(initial);

    // Verify that the response from awaiting _service.GetNotesAsync() is correct.
    // ...
}
```
Running this test will fail on the response verification because we are not yet returning the response of `GetNotesAsync` from the repository. Simply return the response from the call to the repository to make the test pass.

**Note:** The same premise applies to `NotesController` and its use of `INoteService`. Tests will be very similar. Remember to provide the injection for `INoteService` using `NoteService` in `Startup.cs`.

**Git Tag:** [delegating-data-responsibility](https://github.com/xtreme-steve-elliott/NotesApp/tree/delegating-data-responsibility)

**Up Next:** [Introducing an In-Memory Database](../introducing-an-in-memory-database)

**References**  
[The Repository Pattern Explained](http://blog.sapiensworks.com/post/2014/06/02/The-Repository-Pattern-For-Dummies.aspx)  
[Generic Repository Pattern: .net core](https://garywoodfine.com/generic-repository-pattern-net-core/)  
[.NET Dependency Injection](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection)
