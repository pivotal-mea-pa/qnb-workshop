**Previous:** [Introducing an In-Memory Database](../introducing-an-in-memory-database)

In this section, we'll be building out three more endpoints, `GetAsync(long id)`, `PostAsync(Note note)` and `DeleteAsync(long id)`.

### Tips

#### `GetAsync(long id)`

* In `NotesController`, the `GetAsync(long id)` method should be annotated with `[Route("{id}", Name = "GetNoteById")]`. The `Name` attribute will be used to reference the endpoint in the `PostAsync` endpoint.

#### `PostAsync(Note note)`

* For the success case in `PostAsync` of `NotesController`, return a `CreatedAtRouteResult` using:
    ```c#
    CreatedAtRoute("GetNoteById", new { id = processedNote.Id }, processedNote)
    ```
    This makes reference to the `GetAsync(long id)` endpoint that we annotated earlier, and passes the arguments required via an anonymous dictionary, as well as the `Note` itself for the body of the response.
* The response from `PostAsync(Note note)` has a `RouteValues` member that contains all the values from the anonymous dictionary passed to `CreatedAtRoute`.
* Remember to annotate your method parameter on `PostAsync(Note note)` with `[FromBody]`.
* When the `PostAsync(Note note)` endpoint and `AddNoteAsync` methods have been added and tested, they can be used in repository tests to replace calls to `_context.Notes.Add(note)` with `await _repository.AddNoteAsync(note)`.

**Git Tag:** [adding-other-endpoints](https://github.com/xtreme-steve-elliott/NotesApp/tree/adding-other-endpoints)

**Up Next:** [Adding Integration Tests](../adding-integration-tests)

**References**  
[InMemory: Improve in-memory key generation (bug)](https://github.com/aspnet/EntityFrameworkCore/issues/6872)
