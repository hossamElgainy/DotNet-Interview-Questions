# Top 100 C# and .NET Interview Questions & Answers

A comprehensive interview preparation guide for C# and ASP.NET Core backend developers.

**Topics covered:** C#, OOP, LINQ, async programming, .NET internals, ASP.NET Core, Web API, Entity Framework Core, SQL Server, SOLID, design patterns, security, performance, and system design.

---

## Table of Contents

1. [C# Fundamentals (1–15)](#1-c-fundamentals)
2. [Object-Oriented Programming (16–30)](#2-object-oriented-programming)
3. [Collections, LINQ, and Exceptions (31–45)](#3-collections-linq-and-exceptions)
4. [Async Programming and .NET Internals (46–55)](#4-async-programming-and-net-internals)
5. [ASP.NET Core and Web API (56–70)](#5-aspnet-core-and-web-api)
6. [Entity Framework Core and SQL Server (71–82)](#6-entity-framework-core-and-sql-server)
7. [SOLID, Design Patterns, and Architecture (83–92)](#7-solid-design-patterns-and-architecture)
8. [Security, Performance, and System Design (93–100)](#8-security-performance-and-system-design)

---

# 1. C# Fundamentals

## 1. What is the difference between C# and .NET?

**Answer:**

- **C#:** A programming language used to write applications.
- **.NET:** A development platform that provides a runtime, libraries, SDK, and tools.

C# code is typically compiled into Intermediate Language (IL), which the .NET runtime executes using JIT compilation or other supported compilation approaches.

## 2. What is the difference between value types and reference types?

**Answer:**

- Value types include `int`, `bool`, `struct`, and other value-type declarations. Assignment copies the value.
- Reference types include classes, arrays, and delegates. Assignment copies the reference, so multiple variables can refer to the same object.

```csharp
int a = 10;
int b = a;
b = 20;
// a is still 10

var first = new List<int> { 1, 2 };
var second = first;
second.Add(3);
// first now contains 1, 2, 3
```

**Important:** Value types are not always stored on the stack, and reference types are not always stored on the heap. Storage depends on context and runtime implementation.

## 3. What is the difference between `string` and `StringBuilder`?

**Answer:**

`string` is immutable, meaning its contents cannot be changed after creation. `StringBuilder` is mutable and is useful when building text through many incremental modifications.

```csharp
using System.Text;

var builder = new StringBuilder();
builder.Append("Hello");
builder.Append(" World");

string result = builder.ToString();
```

Use `StringBuilder` for repeated concatenation in loops. For simple concatenation or interpolation, a regular string is usually fine.

## 4. What is the difference between `var`, `dynamic`, and `object`?

**Answer:**

- `var`: The compiler infers the variable's static type.
- `object`: Can hold values of any C# type, but specific members generally require casting or pattern matching.
- `dynamic`: Defers member binding to runtime.

```csharp
var number = 10;          // Statically typed as int
object value = "Hello";
dynamic item = "Hello";

Console.WriteLine(((string)value).Length);
Console.WriteLine(item.Length);
```

With `dynamic`, invalid operations may compile and fail at runtime.

## 5. What is the difference between `const` and `readonly`?

**Answer:**

- `const` is a compile-time constant and is implicitly static.
- `readonly` can be assigned at declaration or in the appropriate constructor and can hold a runtime-determined value.

```csharp
public const int MaxRetries = 3;
public readonly DateTime CreatedAt = DateTime.UtcNow;
```

## 6. What is the difference between `ref`, `out`, and `in`?

**Answer:**

- `ref`: Passes a variable by reference; it must be initialized before the call.
- `out`: Passes a variable by reference; the called method must assign it before returning.
- `in`: Passes a readonly reference, useful for avoiding copies of large structs.

```csharp
static void Update(ref int x) => x++;

static void Create(out int x)
{
    x = 10;
}

static int Read(in int x) => x;
```

## 7. What are boxing and unboxing?

**Answer:**

Boxing converts a value type into `object` or a compatible interface representation. Unboxing extracts the value type from a boxed object.

```csharp
int number = 42;

object boxed = number;   // Boxing
int result = (int)boxed; // Unboxing
```

Frequent boxing can cause additional allocations and performance overhead.

## 8. What is the difference between `==` and `.Equals()`?

**Answer:**

`==` is an operator that can be overloaded. `.Equals()` is a method that can be overridden to define equality.

```csharp
string a = "Hello";
string b = "Hello";

Console.WriteLine(a == b);      // True
Console.WriteLine(a.Equals(b)); // True
```

For strings, both compare contents. For many ordinary classes, `==` compares references unless overloaded, while `.Equals()` depends on the implementation.

## 9. What is `Nullable<T>`?

**Answer:**

`Nullable<T>` allows a value type to represent either a value or `null`. The shorthand syntax is `T?`.

```csharp
int? age = null;

int actualAge = age ?? 0;
```

This is useful when a value is optional, such as a middle name's associated numeric field, a missing date, or an optional database column.

## 10. What are nullable reference types?

**Answer:**

Nullable reference types help the compiler identify potential null-reference problems.

```csharp
string name = "Hossam";
string? optionalName = null;
```

The `?` annotation tells the compiler that `null` is an expected possibility. Nullable reference types primarily provide compile-time analysis; they do not automatically add runtime null checks.

## 11. What is the difference between `is`, `as`, and explicit casting?

**Answer:**

- `is`: Checks whether an object matches a type.
- `as`: Attempts a compatible reference or nullable conversion and returns `null` if it fails.
- Explicit casting: Attempts conversion and throws when the conversion is invalid.

```csharp
object value = "Hello";

if (value is string text)
{
    Console.WriteLine(text.Length);
}

string? result = value as string;
string result2 = (string)value;
```

## 12. What are extension methods?

**Answer:**

Extension methods allow you to add method-call syntax to an existing type without modifying its source code.

```csharp
public static class StringExtensions
{
    public static bool HasText(this string? value)
    {
        return !string.IsNullOrWhiteSpace(value);
    }
}

// Usage:
bool valid = "Hello".HasText();
```

They are commonly used for reusable helper functionality and LINQ.

## 13. What are delegates, events, and lambda expressions?

**Answer:**

- **Delegate:** A type-safe reference to methods with a compatible signature.
- **Event:** A publish/subscribe mechanism that lets other objects subscribe to notifications.
- **Lambda:** An inline function expression.

```csharp
Func<int, int> square = x => x * x;

Action<string> print = message =>
    Console.WriteLine(message);
```

Events are useful for notifications, while delegates and lambdas are widely used for callbacks and LINQ.

## 14. What are generics?

**Answer:**

Generics allow classes, interfaces, and methods to work with different types while maintaining compile-time type safety.

```csharp
public class Repository<T>
{
    private readonly List<T> _items = new();

    public void Add(T item) => _items.Add(item);

    public IEnumerable<T> GetAll() => _items;
}
```

Advantages include code reuse, fewer casts, and reduced boxing for value types.

## 15. What is the difference between `IEnumerable<T>` and `IEnumerator<T>`?

**Answer:**

- `IEnumerable<T>` represents a sequence that can be iterated.
- `IEnumerator<T>` represents the current state of an iteration.

An enumerator exposes members such as `MoveNext()` and `Current`. The `foreach` statement generally manages enumerator disposal automatically.

---

# 2. Object-Oriented Programming

## 16. What are the four pillars of OOP?

**Answer:**

1. **Encapsulation:** Protecting internal state and controlling access to it.
2. **Abstraction:** Exposing essential functionality while hiding implementation details.
3. **Inheritance:** Deriving one type from another.
4. **Polymorphism:** Using a common contract for different implementations.

These principles help build reusable and maintainable software.

## 17. What is the difference between an abstract class and an interface?

**Answer:**

| Abstract Class | Interface |
|---|---|
| Can contain instance fields and constructors | Does not provide ordinary instance fields or constructors |
| Can provide shared implementation | Primarily defines a contract |
| A class can inherit from only one class | A class can implement multiple interfaces |
| Useful for shared base behavior | Useful for capabilities and abstractions |

Modern C# interfaces can also provide default implementations and certain static members.

## 18. What is the difference between overloading and overriding?

**Answer:**

**Overloading** means multiple methods have the same name but different parameter signatures.

```csharp
int Add(int a, int b) => a + b;
double Add(double a, double b) => a + b;
```

**Overriding** means a derived class replaces an inherited virtual or abstract implementation.

```csharp
class Animal
{
    public virtual void Speak() => Console.WriteLine("Sound");
}

class Dog : Animal
{
    public override void Speak() => Console.WriteLine("Bark");
}
```

Overloading is resolved at compile time; overriding enables runtime polymorphism.

## 19. What is the difference between `virtual`, `override`, and `new`?

**Answer:**

- `virtual`: Allows a member to be overridden.
- `override`: Replaces an inherited virtual implementation.
- `new`: Hides an inherited member rather than overriding it.

With `new`, the selected member can depend on the reference's compile-time type.

## 20. What is the difference between an abstract class and a sealed class?

**Answer:**

An abstract class cannot be instantiated directly and is intended to be inherited.

A sealed class cannot be inherited.

```csharp
abstract class Animal { }

sealed class PaymentProcessor { }
```

A class cannot be both abstract and sealed in the ordinary class-declaration sense.

## 21. What are constructors and constructor chaining?

**Answer:**

Constructors initialize objects when they are created.

Constructor chaining uses `this()` to call another constructor in the same class or `base()` to call a base-class constructor.

```csharp
class User
{
    public string Name { get; }

    public User() : this("Guest") { }

    public User(string name)
    {
        Name = name;
    }
}
```

## 22. What is the difference between static and instance members?

**Answer:**

Static members belong to the type itself and do not require an object instance.

Instance members belong to individual objects and can access their instance state.

```csharp
class Counter
{
    public static int Total;
    public int Value;
}
```

Static classes cannot be instantiated.

## 23. What is composition, and how does it differ from inheritance?

**Answer:**

Inheritance models an **is-a** relationship. Composition models a **has-a** relationship.

For example, a `Car` has an `Engine`, so composition is usually more appropriate than making `Car` inherit from `Engine`.

Composition often makes dependencies easier to replace, test, and maintain.

## 24. What is Dependency Injection (DI)?

**Answer:**

Dependency Injection supplies a class's dependencies from outside rather than making the class construct them itself.

```csharp
public interface IEmailService
{
    Task SendAsync(string message);
}

public class NotificationService(IEmailService emailService)
{
    public Task NotifyAsync(string message)
        => emailService.SendAsync(message);
}
```

Benefits include loose coupling, improved testability, and easier replacement of implementations.

ASP.NET Core provides a built-in DI container.

## 25. What are association, aggregation, and composition?

**Answer:**

- **Association:** A general relationship between objects.
- **Aggregation:** A whole-part relationship in which the parts can exist independently.
- **Composition:** A stronger ownership relationship in which a part's lifecycle is typically controlled by the whole.

These are object-modeling concepts; C# does not automatically enforce these relationships.

## 26. What is the difference between a class and a struct?

**Answer:**

A class is a reference type, while a struct is a value type.

Structs are often suitable for small, value-like objects. Classes are generally more suitable for objects with identity, shared mutable state, or complex inheritance requirements.

Structs can still contain reference-type fields, so copying a struct does not necessarily deep-copy everything it references.

## 27. What are records in C#?

**Answer:**

Records are types designed for data-centric models and value-based equality.

```csharp
public record Person(string Name, int Age);

var first = new Person("Ali", 25);
var second = new Person("Ali", 25);

Console.WriteLine(first == second); // True
```

`record class` is a reference type, while `record struct` is a value type. Records support `with` expressions but are not automatically deeply immutable.

## 28. What is the difference between shallow copy and deep copy?

**Answer:**

A shallow copy duplicates the outer object while retaining references to nested objects.

A deep copy duplicates the relevant nested object graph.

Deep copying requires deliberate design, especially when objects contain cycles, shared references, or unmanaged resources.

## 29. What are access modifiers in C#?

**Answer:**

Common access modifiers include:

- `public`: Accessible wherever the type is accessible.
- `private`: Accessible within the containing type.
- `protected`: Accessible within the containing type and derived types.
- `internal`: Accessible within the same assembly.
- `protected internal`: Accessible within the same assembly or from derived types.
- `private protected`: Accessible from derived types within the same assembly.

Use the narrowest access level that meets the design requirements.

## 30. What is the difference between `Dispose()` and a finalizer?

**Answer:**

`Dispose()` provides deterministic resource cleanup and is commonly used through `using`.

A finalizer is executed nondeterministically by the runtime, if one is present, before an object is reclaimed.

Use `IDisposable` for resources that require explicit cleanup. Implement a finalizer only when direct unmanaged-resource ownership makes it necessary.

---

# 3. Collections, LINQ, and Exceptions

## 31. What is the difference between an array, `List<T>`, and `LinkedList<T>`?

**Answer:**

- **Array:** Fixed length and efficient indexed access.
- **List<T>:** Resizable array with efficient indexing and amortized-efficient append operations.
- **LinkedList<T>:** Efficient insertion or removal when the relevant node is already known, but no constant-time indexed access.

`List<T>` is the usual choice for most ordered collections.

## 32. What is the difference between `Dictionary<TKey,TValue>` and `Hashtable`?

**Answer:**

`Dictionary<TKey,TValue>` is generic and type-safe. `Hashtable` is a legacy non-generic collection that stores keys and values as objects.

For new C# code, prefer `Dictionary<TKey,TValue>` unless a specific API requires `Hashtable`.

## 33. What is the difference between `List<T>` and `HashSet<T>`?

**Answer:**

A `List<T>` preserves sequence order and allows duplicates.

A `HashSet<T>` stores unique elements according to its equality comparer and typically provides O(1) membership checks.

Use a set when uniqueness or fast existence checks are important.

## 34. What is LINQ?

**Answer:**

LINQ stands for Language Integrated Query. It provides consistent query operations such as filtering, projection, sorting, grouping, and aggregation.

```csharp
var activeNames = users
    .Where(user => user.IsActive)
    .OrderBy(user => user.Name)
    .Select(user => user.Name)
    .ToList();
```

LINQ works with in-memory collections and query providers such as Entity Framework Core.

## 35. What is the difference between `IEnumerable<T>` and `IQueryable<T>`?

**Answer:**

`IEnumerable<T>` supports iteration and commonly executes LINQ-to-Objects operations in .NET.

`IQueryable<T>` represents a query that a provider may translate into another language, such as SQL.

With EF Core, filter and project data before materializing it to reduce unnecessary database work and data transfer.

## 36. What is deferred execution in LINQ?

**Answer:**

Many LINQ operations do not execute until the query is enumerated.

```csharp
var query = numbers.Where(n => n > 10);

// Query executes here:
var result = query.ToList();
```

Deferred execution avoids unnecessary work, but enumerating a query repeatedly may repeat the work or issue multiple database queries.

## 37. What is the difference between `Select()` and `SelectMany()`?

**Answer:**

`Select()` maps each input element to a result.

`SelectMany()` maps each element to a sequence and flattens those sequences into one result sequence.

For example, use `SelectMany()` to retrieve all products from a collection of orders.

## 38. What is the difference between `First()`, `FirstOrDefault()`, `Single()`, and `SingleOrDefault()`?

**Answer:**

- `First()`: Returns the first match; throws if no match exists.
- `FirstOrDefault()`: Returns the first match or a default value.
- `Single()`: Requires exactly one match.
- `SingleOrDefault()`: Allows zero or one match, but throws if multiple matches exist.

Use `Single()` when uniqueness is a requirement you want to enforce.

## 39. What is the difference between `Any()` and `Count()`?

**Answer:**

`Any()` checks whether at least one element matches a condition and can stop at the first match.

`Count()` counts the matching elements.

Prefer `Any()` for existence checks.

```csharp
bool hasAdmins = users.Any(u => u.Role == "Admin");
```

## 40. What is the difference between `GroupBy()`, `Join()`, and `GroupJoin()`?

**Answer:**

- `GroupBy()`: Groups elements according to a key.
- `Join()`: Combines elements from two sequences when their keys match.
- `GroupJoin()`: Associates each outer element with a sequence of matching inner elements.

`GroupJoin()` can be used to build left-join-style results.

## 41. How do `try`, `catch`, and `finally` work?

**Answer:**

- `try`: Contains code that may throw an exception.
- `catch`: Handles an exception of a matching type.
- `finally`: Runs cleanup code whether an exception occurs or not, subject to process termination and similar exceptional circumstances.

```csharp
try
{
    Console.WriteLine("Processing");
}
catch (InvalidOperationException ex)
{
    Console.WriteLine(ex.Message);
}
finally
{
    Console.WriteLine("Finished");
}
```

Do not catch exceptions unless you can handle them, add useful context, or translate them appropriately.

## 42. What is the difference between `throw` and `throw ex`?

**Answer:**

Inside a `catch` block, `throw;` rethrows the current exception while preserving its original stack trace.

`throw ex;` resets the stack trace origin and makes debugging harder.

Prefer `throw;` when rethrowing the same exception.

## 43. How do you create a custom exception?

**Answer:**

Create a class derived from `Exception` when a specific application or domain failure deserves its own type.

```csharp
public sealed class OrderNotFoundException : Exception
{
    public OrderNotFoundException(int id)
        : base($"Order {id} was not found.")
    {
    }
}
```

Avoid creating custom exceptions when a standard exception already describes the situation adequately.

## 44. What is the difference between checked and unchecked arithmetic?

**Answer:**

A `checked` context causes integral arithmetic overflow to throw `OverflowException`.

An `unchecked` context permits the normal overflow behavior of the type.

```csharp
int result = checked(int.MaxValue + 1); // Throws
```

Use checked arithmetic when overflow indicates invalid data or a correctness problem.

## 45. What is `yield return`?

**Answer:**

`yield return` produces sequence elements one at a time without requiring the entire collection to be created in advance.

```csharp
static IEnumerable<int> GetNumbers()
{
    for (int i = 1; i <= 3; i++)
        yield return i;
}
```

The compiler generates an iterator state machine that remembers the current position between iterations.

---

# 4. Async Programming and .NET Internals

## 46. What is the difference between `async` and `await`?

**Answer:**

`async` allows a method or lambda to use `await`.

`await` asynchronously waits for an awaitable operation to complete without blocking the current thread while it waits.

```csharp
public async Task<string> GetDataAsync()
{
    return await httpClient.GetStringAsync(
        "https://example.com");
}
```

Async programming is especially useful for I/O-bound operations. It does not automatically move CPU work to another thread.

## 47. What is the difference between `Task`, `Task<T>`, and `ValueTask<T>`?

**Answer:**

- `Task`: Represents an asynchronous operation without a result.
- `Task<T>`: Represents an asynchronous operation that produces a result.
- `ValueTask<T>`: Can avoid allocations in some cases where a result is frequently available synchronously.

Prefer `Task` and `Task<T>` unless profiling demonstrates a good reason to use `ValueTask<T>`.

## 48. What is the difference between synchronous and asynchronous programming?

**Answer:**

Synchronous code waits for an operation to finish before continuing.

Asynchronous code allows the caller to continue doing other work while an operation such as a database query or HTTP request is pending.

In web applications, asynchronous I/O can improve scalability because threads are not unnecessarily blocked while waiting for external resources.

## 49. What is the difference between concurrency and parallelism?

**Answer:**

- **Concurrency:** Multiple operations can make progress during overlapping periods.
- **Parallelism:** Multiple operations execute at the same instant, usually on different CPU cores.

Async I/O can improve concurrency without requiring parallel CPU execution.

## 50. What is the difference between `Task.WhenAll()` and `Task.WhenAny()`?

**Answer:**

`Task.WhenAll()` completes after all supplied tasks complete.

`Task.WhenAny()` completes when one supplied task completes and returns that task. Other tasks may continue running.

```csharp
Task<int> first = GetFirstAsync();
Task<int> second = GetSecondAsync();

int[] results = await Task.WhenAll(first, second);
```

Use concurrency carefully when operations share limited resources or when cancellation and failures must be coordinated.

## 51. What is the difference between a process and a thread?

**Answer:**

A process is an operating-system execution environment with its own address space and resources.

A thread is an execution path within a process. Threads in one process share the process's memory.

Creating threads has overhead, so asynchronous I/O is often preferable to blocking threads while waiting for I/O.

## 52. What is the CLR?

**Answer:**

The Common Language Runtime executes managed .NET code.

Its responsibilities include:

- Garbage collection.
- Exception handling.
- Runtime type safety.
- JIT compilation.
- Managed execution services.

## 53. How does garbage collection work in .NET?

**Answer:**

The garbage collector automatically reclaims memory used by managed objects that are no longer reachable.

It uses generations to optimize collection:

- **Generation 0:** Commonly contains short-lived objects.
- **Generation 1:** Intermediate generation.
- **Generation 2:** Commonly contains longer-lived objects.

Garbage collection does not replace deterministic disposal of files, database connections, and other resources.

## 54. What is the difference between managed and unmanaged code?

**Answer:**

Managed code executes under the .NET runtime's services.

Unmanaged code executes outside those services, such as native C or C++ code.

.NET applications can interact with native libraries through interoperability mechanisms, but ownership and memory boundaries must be handled carefully.

## 55. What is reflection in C#?

**Answer:**

Reflection allows an application to inspect assemblies, types, members, and attributes at runtime.

It is used in serializers, dependency injection frameworks, plugins, and testing tools.

Reflection can add complexity and overhead and may require extra care in applications using trimming or ahead-of-time compilation.

---

# 5. ASP.NET Core and Web API

## 56. What is the difference between .NET Framework, .NET Core, and modern .NET?

**Answer:**

- **.NET Framework:** Older, Windows-focused implementation.
- **.NET Core:** Cross-platform, modular implementation introduced before the modern .NET naming.
- **Modern .NET:** The unified platform beginning with .NET 5 and continuing through later releases.

New applications generally use a supported modern .NET release unless they depend on Framework-specific technologies.

## 57. What is the ASP.NET Core request pipeline?

**Answer:**

The request pipeline consists of ordered middleware components.

Each component can:

- Process an incoming request.
- Call the next component.
- Short-circuit processing.
- Process the response as it returns.

Middleware ordering is important because components such as routing, authentication, and authorization depend on one another.

## 58. What is middleware, and how do you create custom middleware?

**Answer:**

Middleware processes HTTP requests and responses.

```csharp
public sealed class RequestLoggingMiddleware(
    RequestDelegate next,
    ILogger<RequestLoggingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        logger.LogInformation(
            "Request started: {Path}",
            context.Request.Path);

        await next(context);
    }
}
```

Register it in the application pipeline:

```csharp
app.UseMiddleware<RequestLoggingMiddleware>();
```

Custom middleware is useful for cross-cutting concerns such as logging, request timing, and certain forms of exception handling.

## 59. What is Dependency Injection in ASP.NET Core?

**Answer:**

ASP.NET Core provides a built-in dependency injection container.

Register a service:

```csharp
builder.Services.AddScoped<IOrderService, OrderService>();
```

Inject it into a controller or another service through its constructor:

```csharp
public class OrdersController(
    IOrderService orderService) : ControllerBase
{
}
```

DI centralizes object construction, manages configured lifetimes, and improves testability.

## 60. What is the difference between Transient, Scoped, and Singleton?

**Answer:**

| Lifetime | Behavior | Typical Use |
|---|---|---|
| Transient | New instance each time requested | Lightweight stateless services |
| Scoped | One instance per scope, commonly per HTTP request | Request-level services and `DbContext` |
| Singleton | One instance per service-provider lifetime | Shared, thread-safe services |

**Important:** Do not inject a scoped service directly into a singleton. This creates a captive dependency and can cause incorrect behavior.

Singleton services must also be safe for concurrent use.

## 61. What is the difference between authentication and authorization?

**Answer:**

Authentication determines who the caller is.

Authorization determines what the caller is allowed to do.

For example, a user may successfully authenticate but still receive `403 Forbidden` when trying to access an administrative endpoint without the required permission.

## 62. How does JWT authentication work?

**Answer:**

A JWT is a token that commonly contains claims and is signed by an issuer.

Typical flow:

1. The client authenticates.
2. The authentication service issues an access token.
3. The client sends the token using the `Authorization: Bearer` header.
4. The API validates the token's signature, issuer, audience, expiration, and configured requirements.
5. The API uses validated claims for authorization.

JWT payloads are normally encoded, not encrypted. Never place secrets in a token payload.

## 63. What is the difference between access tokens and refresh tokens?

**Answer:**

An access token is a short-lived credential used to call protected APIs.

A refresh token is used to obtain a new access token without repeating the full authentication process.

Security practices include:

- Secure refresh-token storage.
- Refresh-token rotation.
- Token revocation.
- Reuse detection.
- HTTPS.
- Appropriate expiration and session management.

## 64. What are filters in ASP.NET Core MVC?

**Answer:**

MVC filters run at specific points around MVC action execution.

- **Authorization filters:** Participate in authorization.
- **Resource filters:** Wrap much of the remaining MVC pipeline.
- **Action filters:** Run before and after action execution.
- **Exception filters:** Handle certain unhandled exceptions within MVC.
- **Result filters:** Run around result execution.

Middleware is generally more appropriate for concerns that must apply broadly to the HTTP pipeline.

## 65. What is the difference between MVC and Web API?

**Answer:**

MVC commonly returns rendered views using the Model-View-Controller pattern.

Web APIs expose endpoints that typically return data such as JSON.

ASP.NET Core supports both approaches. API controllers can use `[ApiController]` for API-specific conventions such as automatic model-validation responses.

## 66. What are RESTful APIs and common HTTP methods?

**Answer:**

REST-style APIs expose resources through URLs and use HTTP semantics.

| Method | Common Purpose |
|---|---|
| GET | Retrieve a resource |
| POST | Create a resource or initiate processing |
| PUT | Replace a resource representation |
| PATCH | Partially update a resource |
| DELETE | Remove a resource |

A well-designed API uses appropriate HTTP status codes, resource-oriented URLs, and predictable request/response contracts.

## 67. When should you return HTTP 200, 201, 400, 401, 403, 404, and 500?

**Answer:**

| Status | Meaning |
|---|---|
| 200 OK | Request succeeded |
| 201 Created | Resource created successfully |
| 400 Bad Request | Invalid request or input |
| 401 Unauthorized | Authentication is missing or invalid |
| 403 Forbidden | Caller lacks permission |
| 404 Not Found | Requested resource was not found |
| 500 Internal Server Error | Unexpected server failure |

Do not expose internal stack traces, database details, or secrets in error responses.

## 68. What are model binding and model validation?

**Answer:**

Model binding maps incoming request data to action parameters and model properties.

Model validation checks whether the bound data satisfies validation rules.

```csharp
public class CreateUserRequest
{
    [Required]
    [EmailAddress]
    public string Email { get; set; } = string.Empty;
}
```

With `[ApiController]`, invalid model state normally produces an automatic `400 Bad Request` response.

## 69. What is CORS?

**Answer:**

Cross-Origin Resource Sharing is a browser security mechanism controlled by server response headers.

It determines whether browser scripts from another origin may access a response.

Configure explicit trusted origins, methods, and headers. Do not combine wildcard origins with credentialed requests.

CORS is not a replacement for authentication or authorization.

## 70. How do you implement global exception handling and logging?

**Answer:**

Use centralized exception-handling middleware or `IExceptionHandler` in supported ASP.NET Core versions.

A robust implementation should:

- Log exceptions with structured context.
- Return consistent error responses, such as `ProblemDetails`.
- Distinguish expected application errors from unexpected failures.
- Avoid exposing sensitive implementation details.
- Include a correlation or trace identifier where useful.

---

# 6. Entity Framework Core and SQL Server

## 71. What is Entity Framework Core, and how does it differ from ADO.NET?

**Answer:**

Entity Framework Core is an object-relational mapper (ORM) that maps .NET objects to database records and translates supported LINQ queries into SQL.

ADO.NET provides lower-level database access through connections, commands, and readers.

EF Core often improves productivity, while direct SQL or ADO.NET can be appropriate when more control is required.

## 72. What is the difference between Code First and Database First?

**Answer:**

- **Code First:** Define and maintain models in code, then use migrations to evolve the database schema.
- **Database First:** Start with an existing database and scaffold models and a context from its schema.

Choose based on the project workflow, database ownership, and existing infrastructure.

## 73. What is `DbContext`?

**Answer:**

`DbContext` represents a unit of work and identity map for database operations.

It tracks entity changes, executes queries, and coordinates persistence.

In web applications, it is commonly registered as a scoped service.

**Important:** `DbContext` is not thread-safe. Do not execute concurrent operations on the same context instance.

## 74. What are eager loading, lazy loading, and explicit loading?

**Answer:**

- **Eager loading:** Retrieves related data as part of the initial query, often using `Include()`.
- **Lazy loading:** Retrieves related data when a navigation property is accessed, when configured.
- **Explicit loading:** Retrieves related data through a deliberate additional operation.

Choose carefully to avoid unnecessary data retrieval and the N+1 query problem.

## 75. What is tracking versus `AsNoTracking()`?

**Answer:**

Tracking queries allow EF Core to detect entity changes for persistence and perform identity resolution.

`AsNoTracking()` is often more efficient for read-only queries because returned entities are not tracked by the context.

```csharp
var products = await db.Products
    .AsNoTracking()
    .Where(p => p.IsActive)
    .ToListAsync();
```

Use tracking when you intend to modify entities and save them through the context.

## 76. What are EF Core migrations?

**Answer:**

Migrations represent incremental changes to the database schema.

Typical workflow:

```bash
dotnet ef migrations add AddProductTable
dotnet ef database update
```

Review generated migrations and use a controlled deployment process for production databases. Consider data migrations, rollback strategies, and compatibility between application versions and schemas.

## 77. What is the N+1 query problem?

**Answer:**

The N+1 problem occurs when one query loads a collection and additional queries are issued for individual items.

For example, loading 100 orders and then separately loading each order's customer may cause 101 queries.

Solutions include:

- Appropriate eager loading.
- Projection into DTOs.
- Query restructuring.
- Batching.
- SQL logging and profiling.

## 78. What is the difference between `SaveChanges()` and `SaveChangesAsync()`?

**Answer:**

Both persist tracked changes.

`SaveChangesAsync()` allows the calling thread to perform other work while database I/O is pending, which is beneficial for server scalability.

Use async database APIs consistently in asynchronous request paths and pass cancellation tokens when supported.

## 79. What are database indexes?

**Answer:**

Indexes help databases find rows without scanning an entire table.

Benefits:
- Faster filtering.
- Faster joins.
- Improved sorting and lookup performance.

Costs:
- Additional storage.
- Slower inserts, updates, and deletes.
- Maintenance overhead.

Design indexes based on actual query patterns and execution plans.

## 80. What is the difference between clustered and nonclustered indexes?

**Answer:**

In SQL Server, a clustered index organizes the table's row storage at the leaf level of the index. A table can have at most one clustered index.

A nonclustered index is a separate structure that points to rows.

Choose index keys carefully based on query patterns, key width, uniqueness, and write costs.

## 81. What are transactions, isolation levels, and deadlocks?

**Answer:**

A transaction groups operations so they commit or roll back as a logical unit.

Isolation levels determine how concurrent transactions can observe changes made by one another.

A deadlock occurs when transactions wait on resources held by each other. SQL Server selects a victim transaction to roll back.

Ways to reduce deadlocks include:

- Keeping transactions short.
- Accessing resources in a consistent order.
- Creating appropriate indexes.
- Avoiding unnecessary locks.
- Implementing safe retry policies where appropriate.

## 82. What is the difference between `INNER JOIN`, `LEFT JOIN`, and `FULL OUTER JOIN`?

**Answer:**

- **INNER JOIN:** Returns rows that match on both sides.
- **LEFT JOIN:** Returns every left-side row and matching right-side rows; unmatched right-side columns are `NULL`.
- **FULL OUTER JOIN:** Returns matches and unmatched rows from both sides.

These joins help combine related information across database tables.

---

# 7. SOLID, Design Patterns, and Architecture

## 83. What are the five SOLID principles?

**Answer:**

### S — Single Responsibility Principle
A module should have one coherent reason to change.

### O — Open/Closed Principle
Software entities should be open to extension but closed to unnecessary modification.

### L — Liskov Substitution Principle
A subtype should be usable wherever its base type is expected without breaking correctness.

### I — Interface Segregation Principle
Prefer focused interfaces over large interfaces that force clients to depend on methods they do not use.

### D — Dependency Inversion Principle
High-level policy should depend on abstractions rather than concrete low-level implementations.

## 84. What is the difference between Dependency Inversion and Dependency Injection?

**Answer:**

Dependency Inversion is a design principle about the direction of dependencies.

Dependency Injection is a technique for supplying dependencies from outside a class.

DI can help implement Dependency Inversion, but the two concepts are not identical.

## 85. What is the Repository Pattern? Is it always needed with EF Core?

**Answer:**

The Repository Pattern abstracts data access behind a collection-like interface or domain-specific query operations.

Benefits can include:
- Isolation of persistence details.
- Clear domain boundaries.
- Focused data-access contracts.
- Easier testing of application logic.

However, EF Core's `DbContext` and `DbSet` already provide some repository and unit-of-work behavior. Avoid adding generic repositories that merely duplicate EF Core without providing useful abstraction.

## 86. What is the Unit of Work Pattern?

**Answer:**

Unit of Work coordinates changes that should be persisted as one logical operation.

EF Core's `DbContext` already acts as a unit of work for tracked changes.

Use explicit transactions when multiple database operations must commit or roll back atomically.

## 87. What is the difference between Clean Architecture, Onion Architecture, and Layered Architecture?

**Answer:**

All three approaches aim to separate responsibilities.

**Layered Architecture:** Commonly separates presentation, business logic, and data access.

**Onion Architecture:** Places domain logic at the center, with dependencies pointing inward.

**Clean Architecture:** Emphasizes independent business rules and inward dependency direction, with infrastructure details kept outside the core.

In Onion and Clean Architecture, the domain should not depend directly on infrastructure implementations.

## 88. What are Factory, Strategy, and Singleton patterns?

**Answer:**

**Factory Pattern:** Encapsulates object creation decisions.

**Strategy Pattern:** Makes interchangeable algorithms available behind a common interface.

**Singleton Pattern:** Ensures a single instance of a type or service registration within its intended scope.

In ASP.NET Core, a singleton DI lifetime means one instance per service provider, not necessarily one universal instance across every process.

## 89. What is CQRS?

**Answer:**

CQRS stands for Command Query Responsibility Segregation.

It separates:
- **Commands:** Operations that change state.
- **Queries:** Operations that read data.

CQRS can help complex systems optimize read and write paths independently, but it introduces complexity. It is not necessary for every CRUD application.

## 90. What is the difference between a monolithic application and microservices?

**Answer:**

A monolith packages much of an application into one deployable unit.

Microservices split application capabilities into independently deployable services.

Microservices can provide independent scaling and deployment, but introduce network failures, distributed data consistency, operational complexity, and additional observability requirements.

Start with clear modular boundaries and adopt microservices when there is a justified need.

## 91. What is the difference between a DTO, an entity, and a view model?

**Answer:**

- **Entity:** Represents a domain concept or persisted object, often with identity.
- **DTO:** Transfers data across a boundary, such as an API request or response.
- **View model:** Shapes data for a particular UI or view.

Separating these types can prevent database implementation details from leaking into public API contracts.

## 92. What is the difference between unit testing, integration testing, and mocking?

**Answer:**

**Unit Testing:** Verifies a small unit in isolation.

**Integration Testing:** Verifies that components work together, such as an API and database.

**Mocking:** Uses a test double to simulate a dependency or verify interactions. It is a testing technique rather than a separate test category.

Use unit tests for isolated logic and integration tests for important database, API, and infrastructure behavior.

---

# 8. Security, Performance, and System Design

## 93. How do you prevent SQL injection?

**Answer:**

Use parameterized queries and EF Core LINQ instead of concatenating untrusted input into SQL strings.

```csharp
var user = await db.Users
    .FirstOrDefaultAsync(
        u => u.Email == email,
        cancellationToken);
```

If raw SQL is necessary, use parameter APIs correctly. Dynamic identifiers such as column names should be selected from a strict allowlist.

Also apply least-privilege database permissions.

## 94. How do you securely store passwords and secrets?

**Answer:**

For passwords:
- Never store plaintext passwords.
- Use a proven password-hashing implementation, such as ASP.NET Core Identity's password hasher.
- Use appropriate salt and work-factor settings.
- Apply rate limiting and suitable authentication protections.

For secrets:
- Never commit production secrets to source control.
- Use a secret manager or deployment secret store.
- Restrict access.
- Rotate credentials when necessary.
- Avoid logging passwords, tokens, or connection secrets.

## 95. What is caching? What is the difference between in-memory and distributed caching?

**Answer:**

Caching temporarily stores data to reduce repeated work or latency.

**In-memory caching:** Data is stored within one application instance.

**Distributed caching:** Data is stored in a shared service, such as Redis, that multiple application instances can access.

Use suitable expiration and invalidation policies. Avoid caching sensitive user data under shared keys.

## 96. How would you optimize a slow ASP.NET Core API?

**Answer:**

Use a measurement-driven process:

1. Measure latency, throughput, errors, CPU, and memory.
2. Inspect database queries and execution plans.
3. Identify N+1 queries and missing or inefficient indexes.
4. Select only required columns and implement pagination.
5. Use async I/O appropriately.
6. Cache frequently requested data when safe.
7. Reduce unnecessary serialization and allocations.
8. Inspect slow external service calls.
9. Load-test changes and compare results.

**Interview tip:** Explain how you would identify the bottleneck before proposing a solution.

## 97. How would you implement pagination, filtering, and sorting for a large dataset?

**Answer:**

Apply filtering, ordering, and projection in the database query before materializing results.

```csharp
var items = await db.Orders
    .Where(o => o.CustomerId == customerId)
    .OrderByDescending(o => o.CreatedAt)
    .ThenByDescending(o => o.Id)
    .Select(o => new OrderDto(
        o.Id,
        o.CreatedAt,
        o.Total))
    .Take(pageSize)
    .ToListAsync(cancellationToken);
```

For offset pagination, use `Skip()` and `Take()` with a stable ordering.

For large datasets or frequently changing records, keyset/cursor pagination may perform better than deep offset pagination.

Always:
- Validate page size.
- Allowlist sortable fields.
- Validate filters.
- Avoid returning unnecessary columns.
- Consider indexes that support the query.

## 98. How would you handle two users updating the same record simultaneously?

**Answer:**

Use optimistic concurrency when conflicts are relatively uncommon.

With EF Core and SQL Server, a `rowversion` column can be configured as a concurrency token.

If another user modifies the row first, saving changes can raise `DbUpdateConcurrencyException`.

Possible responses include:
- Asking the user to reload.
- Merging changes.
- Retrying safely.
- Rejecting the update with a conflict response.

Choose the policy according to the business requirements.

## 99. How would you design an API that handles one million users?

**Answer:**

First clarify what one million means:
- One million registered users?
- One million daily active users?
- One million concurrent users or requests?

These scenarios require very different capacity planning.

A scalable architecture may include:

1. **Load balancing:** Distribute requests across multiple ASP.NET Core instances.
2. **Stateless application instances:** Avoid relying on local server memory for shared session state.
3. **Database optimization:** Use appropriate indexes, query tuning, and connection management.
4. **Pagination:** Limit result sizes and avoid unbounded queries.
5. **Caching:** Use distributed caching where the data's consistency requirements allow it.
6. **Background processing:** Move suitable slow tasks to queues and workers.
7. **Resilience:** Use timeouts, cancellation, rate limiting, and controlled retries.
8. **Observability:** Collect logs, metrics, traces, and alerts.
9. **Security:** Apply authentication, authorization, and least privilege.
10. **Capacity planning:** Load-test realistic workloads and scale based on measured bottlenecks.

Do not introduce microservices or other distributed components without a clear reason.

## 100. How would you design secure authentication using JWT, refresh tokens, roles, and permissions?

**Answer:**

A robust design includes the following:

**1. Authentication**
- Validate credentials securely.
- Store passwords using a proven password hasher.
- Apply rate limits to sensitive authentication endpoints.

**2. Access tokens**
- Issue short-lived signed access tokens.
- Validate issuer, audience, signature, and expiration.
- Avoid placing confidential data in JWT payloads.

**3. Refresh tokens**
- Generate sufficiently random refresh tokens.
- Store them securely, often as hashes on the server.
- Rotate tokens when used.
- Detect reuse and revoke affected sessions.
- Support expiration and revocation.

**4. Authorization**
- Use roles and claims for identity-based access.
- Use policies for more granular permissions.
- Enforce authorization on the server for every protected operation.

**5. Operational security**
- Require HTTPS.
- Protect signing keys and secrets.
- Audit relevant security events.
- Support session management and token revocation where needed.

A valid JWT does not automatically mean that a user has permission to perform every operation.

---

# Recommended Study Order

For a backend .NET developer, I recommend this order:

1. **C# Fundamentals and OOP:** Questions 1–30.
2. **Collections and LINQ:** Questions 31–45.
3. **Async Programming:** Questions 46–55.
4. **ASP.NET Core, DI, and Authentication:** Questions 56–70.
5. **EF Core and SQL Server:** Questions 71–82.
6. **SOLID and Architecture:** Questions 83–92.
7. **Security and Real-World Scenarios:** Questions 93–100.

## How to Answer Interview Questions

For each question, prepare four things:

1. **Definition:** Explain the concept in 2–4 sentences.
2. **Comparison:** Explain how it differs from related concepts.
3. **Example:** Give a short code example or real project scenario.
4. **Trade-offs:** Explain when you would or would not use it.

## Official Learning Resources

- [C# Documentation](https://learn.microsoft.com/en-us/dotnet/csharp/)
- [ASP.NET Core Fundamentals](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/)
- [ASP.NET Core Learning Path](https://learn.microsoft.com/en-us/training/paths/aspnet-core-fundamentals/)
- [Entity Framework Core Documentation](https://learn.microsoft.com/en-us/ef/core/)
- [C# Asynchronous Programming](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/)
- [SQL Server Documentation](https://learn.microsoft.com/en-us/sql/sql-server/)

---

*This guide is intended for interview preparation. Check version-specific APIs and practices against the .NET version used by the target role.*
