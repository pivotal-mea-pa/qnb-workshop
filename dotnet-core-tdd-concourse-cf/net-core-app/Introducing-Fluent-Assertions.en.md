**Previous:** [Creating a RESTful API](../creating-a-restful-api)

You may have heard of the concept of fluent assertions before with [AssertJ](http://joel-costigliola.github.io/assertj/) for Java. The underlying goal is to make your testing assertions read like a sentence, as this greatly improves the speed at which someone can understand exactly what you're trying to test.

For .NET, fluent assertions come in the form of the aptly named library [FluentAssertions](https://fluentassertions.com/documentation/).

One thing that is really nice about this library is the ability to chain multiple checks together if they share the same subject, using `.And`.

***

Integration into our current setup is fairly simple. Start by going into the NuGet dependencies for `NotesApp.Tests` and searching for `FluentAssertions`. As of the writing of this guide, the latest version is `5.6.0`.

Let's go change that test now.

Previously, we had the following:
```c#
Assert.NotNull(actual);
Assert.NotEmpty(actual);
Assert.Equal(actual[0].Id, expected[0].Id);
Assert.Equal(actual[0].Body, expected[0].Body);
```

With FluentAssertions, this can be rewritten in a few different ways:
```c#
// The most literal translation
actual.Should().NotBeNull();
actual.Should().NotBeEmpty();
actual[0].Id.Should().BeEquivalentTo(expected[0].Id);
actual[0].Body.Should().BeEquivalentTo(expected[0].Body);

// Joining assertions and using list equivalence
actual.Should().NotBeNullOrEmpty().And.BeEquivalentTo(expected);
```

**Note:** `x.Should().BeEquivalentTo(y)` can be told to include/exclude certain properties and perform checks in many different ways.

For example:
```c#
x.Should().BeEquivalentTo(y, opt => opt.Excluding(n => n.Id));
```
Tells it that `x` should be equivalent to `y` except for the `Id` property.

**Git Tag:** [introducing-fluent-assertions](https://github.com/xtreme-steve-elliott/NotesApp/tree/introducing-fluent-assertions)

**Up Next:** [Making the App Asynchronous](../making-the-app-asynchronous)
