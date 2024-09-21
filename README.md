![image](https://github.com/user-attachments/assets/79398670-a25c-407e-94e0-9a2b321d14d5)# Laravel and Gang of Four (GoF) Design Patterns

![image](https://github.com/user-attachments/assets/0e78cd1e-3d75-4eb8-854a-8c6e085336fc)


| **#** | **Design Pattern**         | **Laravel Example**                    | **Category**         |
|-------|----------------------------|----------------------------------------|----------------------|
| 1     | [Abstract Factory](#1-abstract-factory)            | Cache Manager                          | Creational Patterns  |
| 2     | [Builder](#2-builder)                     | Query Builder                          | Creational Patterns  |
| 3     | [Factory Method](#3-factory-method)              | Service Container's `make` Method      | Creational Patterns  |
| 4     | [Prototype](#4-prototype)                   | Model Replication                      | Creational Patterns  |
| 5     | [Singleton](#5-singleton)                   | Application Instance                   | Creational Patterns  |
| 6     | [Adapter](#6-adapter)                     | Cache and Filesystem Adapters          | Structural Patterns  |
| 7     | [Bridge](#7-bridge)                      | Queue System                           | Structural Patterns  |
| 8     | [Composite](#8-composite)                   | Blade Templates                        | Structural Patterns  |
| 9     | [Decorator](#9-decorator)                   | Middleware                             | Structural Patterns  |
| 10    | [Facade](#10-facade)                      | Facade Classes                         | Structural Patterns  |
| 11    | [Flyweight](#11-flyweight)                   | View Caching                           | Structural Patterns  |
| 12    | [Proxy](#12-proxy)                       | Eloquent Relationships                 | Structural Patterns  |
| 13    | [Chain of Responsibility](#13-chain-of-responsibility)     | HTTP Kernel Middleware Stack           | Behavioral Patterns  |
| 14    | [Command](#14-command)                     | Artisan Commands and Jobs              | Behavioral Patterns  |
| 15    | [Interpreter](#15-interpreter)                 | Blade Template Engine                  | Behavioral Patterns  |
| 16    | [Iterator](#16-iterator)                    | Collections and Eloquent Results       | Behavioral Patterns  |
| 17    | [Mediator](#17-mediator)                    | Event Dispatcher                       | Behavioral Patterns  |
| 18    | [Memento](#18-memento)                     | Database Transactions                  | Behavioral Patterns  |
| 19    | [Observer](#19-observer)                    | Eloquent Model Events                  | Behavioral Patterns  |
| 20    | [State](#20-state)                       | Order Processing                       | Behavioral Patterns  |
| 21    | [Strategy](#21-strategy)                    | Authentication Guards                  | Behavioral Patterns  |
| 22    | [Template Method](#22-template-method)             | `Mailable` Classes                     | Behavioral Patterns  |
| 23    | [Visitor](#23-visitor)                     | Validation                             | Behavioral Patterns  |

---

[Go Back to Top](#)


<small><small><small>

## **Creational Patterns**

### **1. Abstract Factory**

**Example in Laravel:** **Cache Manager**

Laravel's Cache system supports multiple cache drivers (e.g., file, database, Redis, Memcached). The `CacheManager` class acts as an Abstract Factory that creates cache store instances based on the configuration.

**Implementation:**

- **CacheManager:** Determines which cache driver to use.
- **Cache Stores:** Concrete implementations like `FileStore`, `RedisStore`, etc.

**Code Example:**

```php
// config/cache.php
'default' => env('CACHE_DRIVER', 'file'),

// Using the cache
Cache::put('key', 'value', $minutes);
$value = Cache::get('key');
```

Here, `Cache::get()` uses the `CacheManager` to return the appropriate cache store instance without the application needing to know which cache driver is being used.

---

### **2. Builder**

**Example in Laravel:** **Query Builder**

Laravel's Query Builder provides a fluent interface to build SQL queries step by step. It abstracts the creation of complex queries by chaining methods.

**Implementation:**

- **Query Builder Class:** Acts as the Director.
- **Fluent Methods:** Each method adds parts to the query.

**Code Example:**

```php
$users = DB::table('users')
    ->where('status', 'active')
    ->orderBy('created_at', 'desc')
    ->get();
```

Each method (`where`, `orderBy`, `get`) adds to the query construction, resulting in an efficient and readable query-building process.

---

### **3. Factory Method**

**Example in Laravel:** **Service Container's `make` Method**

Laravel's Service Container uses the Factory Method pattern to create objects. When you resolve a class from the container, it instantiates the class, injecting dependencies as needed.

**Implementation:**

- **Container (`Illuminate\Container\Container`):** Creator class.
- **`make` Method:** Factory method that creates instances.

**Code Example:**

```php
$service = app()->make('App\Services\ReportService');
```

This code tells the container to create an instance of `ReportService`, resolving any dependencies automatically.

---

### **4. Prototype**

**Example in Laravel:** **Model Replication**

Laravel's Eloquent models provide a `replicate` method to create a copy of an existing model instance.

**Implementation:**

- **`replicate` Method:** Clones the object.
- **New Instance:** Can be modified and saved as a new record.

**Code Example:**

```php
$originalUser = User::find(1);
$newUser = $originalUser->replicate();
$newUser->email = 'newuser@example.com';
$newUser->save();
```

This creates a new user with the same attributes as the original, except for the modified email.

---

### **5. Singleton**

**Example in Laravel:** **Application Instance**

Laravel's Application class (`Illuminate\Foundation\Application`) is a singleton that represents the service container. It ensures there's only one instance throughout the application's lifecycle.

**Implementation:**

- **Singleton Binding:** Using `$this->app->singleton()` in service providers.
- **Global Helper:** The `app()` function returns the singleton instance.

**Code Example:**

```php
$appInstance1 = app();
$appInstance2 = app();

var_dump($appInstance1 === $appInstance2); // Outputs: bool(true)
```

This confirms that both variables point to the same instance.

---

## **Structural Patterns**

### **6. Adapter**

**Example in Laravel:** **Cache and Filesystem Adapters**

Laravel uses adapters to provide a consistent interface to different cache and filesystem drivers.

**Implementation:**

- **Adapter Classes:** Translate the interface of a class into another interface.
- **Cache Stores:** Each store adapts its specific implementation to the cache interface.

**Code Example:**

```php
// Using Redis Cache
Cache::store('redis')->put('key', 'value', $seconds);

// Using File Cache
Cache::store('file')->put('key', 'value', $seconds);
```

Both stores provide the same interface (`put`, `get`), even though they use different underlying systems.

---

### **7. Bridge**

**Example in Laravel:** **Queue System**

Laravel's Queue system decouples the abstraction of queueing jobs from the actual queue implementation (e.g., Beanstalkd, Amazon SQS, Redis).

**Implementation:**

- **Abstraction:** Queueing jobs using a unified interface.
- **Implementation:** Specific drivers handle the actual queuing.

**Code Example:**

```php
// Dispatching a job
dispatch(new SendEmailJob($user));

// Queue configuration in config/queue.php
'default' => env('QUEUE_CONNECTION', 'sync'),
```

Changing the `QUEUE_CONNECTION` value switches the underlying queue implementation without altering the job dispatching code.

---

### **8. Composite**

**Example in Laravel:** **Blade Templates**

Blade templates allow you to compose views using layouts and sections, forming a part-whole hierarchy.

**Implementation:**

- **Components and Slots:** Build complex views from simple components.
- **Layouts:** Define a base structure that child views extend.

**Code Example:**

```blade
<!-- resources/views/layouts/app.blade.php -->
<html>
<head>
    <title>@yield('title')</title>
</head>
<body>
    @yield('content')
</body>
</html>

<!-- resources/views/home.blade.php -->
@extends('layouts.app')

@section('title', 'Home Page')

@section('content')
    <h1>Welcome to the Home Page</h1>
@endsection
```

The `home.blade.php` view extends the layout and fills in the sections, composing the final view.

---

### **9. Decorator**

**Example in Laravel:** **Middleware**

Middleware wraps the HTTP request and response, adding additional behavior without modifying the core request handling.

**Implementation:**

- **Middleware Classes:** Decorate the request/response.
- **`handle` Method:** Calls the next middleware in the stack.

**Code Example:**

```php
public function handle($request, Closure $next)
{
    // Pre-processing
    if ($request->has('blocked')) {
        return response('Forbidden', 403);
    }

    $response = $next($request);

    // Post-processing
    $response->header('X-Custom-Header', 'Value');

    return $response;
}
```

This middleware adds behavior before and after the request is handled by the application.

---

### **10. Facade**

**Example in Laravel:** **Facade Classes**

Laravel's Facades provide a static interface to classes in the service container, acting as a proxy to underlying classes.

**Implementation:**

- **Facade Classes:** Extend `Illuminate\Support\Facades\Facade`.
- **`getFacadeAccessor` Method:** Defines the service in the container.

**Code Example:**

```php
// Using the Cache Facade
Cache::put('key', 'value', $minutes);
$value = Cache::get('key');
```

The `Cache` facade provides a static interface to the underlying cache service.

---

### **11. Flyweight**

**Example in Laravel:** **View Caching**

Laravel caches compiled Blade templates to improve performance, sharing the compiled views across requests.

**Implementation:**

- **Flyweight Objects:** Shared instances of compiled views.
- **Caching Mechanism:** Stores compiled templates to reduce memory usage.

**Code Example:**

```php
// Compiled views are stored in storage/framework/views
// Laravel checks if a compiled view exists before recompiling
```

This reduces the overhead of compiling views on every request.

---

### **12. Proxy**

**Example in Laravel:** **Eloquent Relationships**

Eloquent uses lazy loading for relationships, acting as a proxy that delays the loading of related models until they're needed.

**Implementation:**

- **Proxy Object:** Placeholder for the related models.
- **Lazy Loading:** Retrieves data when the property is accessed.

**Code Example:**

```php
$user = User::find(1);
$posts = $user->posts; // The 'posts' relationship is loaded on demand
```

Here, `$user->posts` acts as a proxy to the actual posts, fetching them when accessed.

---

## **Behavioral Patterns**

### **13. Chain of Responsibility**

**Example in Laravel:** **HTTP Kernel Middleware Stack**

Laravel's middleware stack passes the HTTP request through a chain of middleware classes.

**Implementation:**

- **Handlers (Middleware):** Each can handle the request or pass it to the next handler.
- **Request Flow:** The request passes through the chain in order.

**Code Example:**

```php
// app/Http/Kernel.php
protected $middleware = [
    \App\Http\Middleware\CheckForMaintenanceMode::class,
    \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
    // ...
];
```

Each middleware can decide whether to handle the request or pass it along.

---

### **14. Command**

**Example in Laravel:** **Artisan Commands and Jobs**

Laravel's Artisan commands and queued jobs encapsulate actions as objects.

**Implementation:**

- **Command Objects:** Classes that implement a specific action.
- **Invoker:** The Artisan console or queue worker executes the command.

**Code Example (Artisan Command):**

```php
// app/Console/Commands/SendEmails.php
class SendEmails extends Command
{
    protected $signature = 'email:send';

    public function handle()
    {
        // Command logic
    }
}
```

**Code Example (Job):**

```php
// app/Jobs/ProcessPodcast.php
class ProcessPodcast implements ShouldQueue
{
    public function handle()
    {
        // Job logic
    }
}
```

---

### **15. Interpreter**

**Example in Laravel:** **Blade Template Engine**

Laravel's Blade engine interprets the Blade syntax into PHP code.

**Implementation:**

- **Grammar:** Blade directives (e.g., `@if`, `@foreach`).
- **Interpreter:** Parses and compiles Blade templates into PHP.

**Code Example:**

```blade
@if($user->isAdmin())
    <p>Welcome, admin!</p>
@endif
```

Blade compiles this into PHP code that can be executed by the server.

---

### **16. Iterator**

**Example in Laravel:** **Collections and Eloquent Results**

Laravel's `Collection` class implements the `IteratorAggregate` interface, allowing iteration over its items.

**Implementation:**

- **Aggregate Object:** The `Collection` that contains items.
- **Iterator:** Provides a way to access elements sequentially.

**Code Example:**

```php
$users = User::where('active', true)->get();

foreach ($users as $user) {
    echo $user->name;
}
```

You can iterate over `$users` because `Collection` implements the iterator interfaces.

---

### **17. Mediator**

**Example in Laravel:** **Event Dispatcher**

Laravel's event system acts as a mediator between event sources and listeners.

**Implementation:**

- **Mediator (`Event Dispatcher`):** Manages communication between objects.
- **Colleagues:** Event listeners and subscribers.

**Code Example:**

```php
// Firing an event
event(new OrderShipped($order));

// Listening for the event
Event::listen(OrderShipped::class, function ($event) {
    // Handle the event
});
```

The event dispatcher mediates between the event source and listeners.

---

### **18. Memento**

**Example in Laravel:** **Database Transactions**

Laravel's database transactions allow you to save the state of the database and roll back to it if needed.

**Implementation:**

- **Memento:** The state of the database at a point in time.
- **Originator:** The database connection.
- **Caretaker:** Manages the saving and restoring of state.

**Code Example:**

```php
DB::transaction(function () {
    // Perform database operations
    // If an exception occurs, the transaction is rolled back
});
```

The transaction acts as a memento, allowing the database to revert to its previous state in case of failure.

---

### **19. Observer**

**Example in Laravel:** **Eloquent Model Events**

Laravel models can fire events such as `creating`, `updating`, and `deleting`, which observers can listen to.

**Implementation:**

- **Subject:** The Eloquent model.
- **Observers:** Classes that respond to events.

**Code Example:**

```php
// Defining an observer
class UserObserver
{
    public function created(User $user)
    {
        // Send a welcome email
    }
}

// Registering the observer
User::observe(UserObserver::class);
```

---

### **20. State**

**Example in Laravel:** **Order Processing**

An Eloquent model can represent different states, altering behavior based on the current state.

**Implementation:**

- **Context:** The model (e.g., `Order`).
- **State Objects:** Different classes or properties representing states (e.g., `Pending`, `Processing`, `Shipped`).

**Code Example:**

```php
class Order extends Model
{
    public function ship()
    {
        if ($this->status !== 'processing') {
            throw new Exception('Order cannot be shipped.');
        }

        $this->status = 'shipped';
        $this->save();
    }
}
```

The `Order` model changes behavior based on its `status` property.

---

### **21. Strategy**

**Example in Laravel:** **Authentication Guards**

Laravel supports multiple authentication strategies (guards), which can be swapped based on configuration.

**Implementation:**

- **Strategy Interface:** Defines methods for authentication.
- **Concrete Strategies:** Implement different authentication methods (e.g., session, token).

**Code Example:**

```php
// config/auth.php
'guards' => [
    'web' => [
        'driver' => 'session',
        'provider' => 'users',
    ],
    'api' => [
        'driver' => 'token',
        'provider' => 'users',
    ],
],

// Using the guard
Auth::guard('api')->attempt($credentials);
```

---

### **22. Template Method**

**Example in Laravel:** **`Mailable` Classes**

Laravel's `Mailable` classes define a `build` method that outlines the steps to compose an email, which subclasses implement.

**Implementation:**

- **Abstract Class (`Mailable`):** Provides the template method.
- **Concrete Class:** Implements the `build` method.

**Code Example:**

```php
class WelcomeEmail extends Mailable
{
    public function build()
    {
        return $this->view('emails.welcome')
                    ->subject('Welcome to Our Platform');
    }
}
```

The `build` method is the template method that defines how the email is constructed.

---

### **23. Visitor**

**Example in Laravel:** **Validation**

Laravel's validation can be seen as a visitor that checks the request data against a set of rules.

**Implementation:**

- **Visitor:** The validator that performs operations on elements.
- **Element:** The data being validated.

**Code Example:**

```php
$request->validate([
    'email' => 'required|email',
    'password' => 'required|min:8',
]);
```

The validator "visits" each data element to apply validation rules.

**Note:** The Visitor pattern isn't commonly used explicitly in Laravel's core, but the validation mechanism resembles its behavior.

---

## **Summary**

Laravel effectively utilizes all 23 GoF design patterns throughout its framework and ecosystem:

- **Creational Patterns:** Used in services creation, dependency injection, and object cloning.
- **Structural Patterns:** Applied in adapting interfaces, composing views, and providing unified interfaces.
- **Behavioral Patterns:** Employed in request handling, event management, state management, and algorithms definition.

Understanding these patterns within Laravel can help developers recognize design decisions, enhance code maintainability, and apply best practices in their own projects.

---

**Additional Resources:**

- **Laravel Documentation:** [https://laravel.com/docs](https://laravel.com/docs)
- **Design Patterns in PHP:** [https://designpatternsphp.readthedocs.io](https://designpatternsphp.readthedocs.io)
- **Gang of Four Design Patterns:** [https://en.wikipedia.org/wiki/Design_Patterns](https://en.wikipedia.org/wiki/Design_Patterns)

</small></small></small>
