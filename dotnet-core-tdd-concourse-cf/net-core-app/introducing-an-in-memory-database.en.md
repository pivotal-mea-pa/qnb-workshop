# Introducing an In-Memory Database

**Previous:** [Delegating Data Responsibility](../delegating-data-responsibility)

In this section, we'll make use of an in-memory database to store our `Note` objects.

### Adding a Database Context
In order to interact with the database that will be added, we will need to have a database context/session (extending `DbContext`) that incorporates a queryable/modifiable collection.

Start by adding a new class, `NotesAppDbContext`, to the `Models` folder:
```c#
public class NotesAppDbContext : DbContext
{
    public DbSet<Note> Notes { get; set; }
    
    public NotesAppDbContext(DbContextOptions<NotesAppDbContext> options) : base(options) { }
}
```

### Injecting the Context
In the previous section, we delegated responsibility for data management to the `NoteService` and `NoteRepository`. To access the data, we need to provide `NoteRepository` with the `NotesAppDbContext` we just created.

Open `NoteRepository.cs` and add a constructor that takes a `NotesAppDbContext`, storing it as `_context`, and calls `context.Database.EnsureCreated();`. The `EnsureCreated` call ensures that the tables in the database are properly created before we try to access them.

To provide this context, we must go to `Startup.cs` and register it. Add the following above the line where we registered the `INoteRepository`:
```c#
services.AddDbContext<NotesAppDbContext>(ConfigureDbOptions);
```
The method `ConfigureDbOptions` is separated so it can be overridden easily when we get to integration tests.
```c#
protected virtual void ConfigureDbOptions(DbContextOptionsBuilder options) {
    options.UseInMemoryDatabase("notes_app");
}
```

Now we need to update our tests to reflect the new constructor. Open up `NoteRepositoryTests.cs` and add the following:
```c#
private static int _testDbIndex;
private readonly NotesAppDbContext _context;

public NoteRepositoryTests()
{
    // Make a new database (increment to make a new name)
    var dbContextOptions = new DbContextOptionsBuilder<NotesAppDbContext>()
        .UseInMemoryDatabase($"note_repository_tests{_testDbIndex++}")
        .Options;
    _context = new NotesAppDbContext(dbContextOptions);
    _repository = new NoteRepository(_context);
}
```

### Adjusting Our Tests
Make the `NoteRepositoryTests` class implement the `IDisposable` pattern to clean up the instances of the database after the test has been run.
```c#
public void Dispose()
{
    _context?.Database?.EnsureDeleted();
    _context?.Dispose();
}
```

Rename `GetNotes_ShouldReturnNoteList` to `GetNotesAsync_WhenNotes_ShouldReturnNoteList` and add the following setup before the call to the repository:
```c#
{ // Setup for test until we get around to having an Add method in the service
    _context.Notes.Add(expected[0]);
    _context.SaveChanges();
}
```

Add another test, `GetNotesAsync_WhenNoNotes_ShouldReturnEmptyList` to validate the data when there is no setup.

We'll need to get the tests to pass using the data we've set them up with, so open up `NoteRepository.cs`. To make the repository use data from the database, the `Notes` object in the `NotesAppDbContext` will need to be accessed. In `GetNotesAsync()`, simply return the data.

Change
```c#
new[] { new Note { Id = 1, Body = "Note 1" } }.AsEnumerable()
```
to
```c#
_context.Notes.AsEnumerable() ?? Enumerable.Empty<Note>()
```
If obtaining the enumerable returns null, we instead return an empty enumerable.

At this point, run your tests again. They should pass.

**Git Tag:** [introducing-an-in-memory-database](https://github.com/xtreme-steve-elliott/NotesApp/tree/introducing-an-in-memory-database)

**Up Next:** [Adding Other Endpoints](../adding-other-endpoints)

**References**  
[Configuring DbContext](https://docs.microsoft.com/en-us/ef/core/miscellaneous/configuring-dbcontext)  
[IDisposable Interface](https://docs.microsoft.com/en-us/dotnet/api/system.idisposable?view=netcore-2.2)
