# Laravel Interview Notebook (Mid to Senior Level)
**100 Essential Questions & Answers**

---

## 1. Core Architecture & Request Lifecycle

### Q1. Can you describe the Laravel request lifecycle?
* **Answer:** An HTTP request enters the entry point at `public/index.php`. It is loaded by Composer's autoloader, then instances of the application and the HTTP or Console Kernel are created. The kernel handles bootstrapping tasks (loading configuration, registering service providers), routes the request through middleware stacks, hits a controller or closure, and returns a response object back through the middleware stack to the client.

### Q2. What is the Service Container in Laravel?
* **Answer:** The Service Container is a powerful tool for managing class dependencies and performing dependency injection. It acts as a registry to track classes and bind interfaces to concrete implementations.

### Q3. What is the difference between the `register` and `boot` methods in Service Providers?
* **Answer:** The `register` method is strictly used to bind things into the service container; you should never attempt to listen for events, route, or use other services here because all providers might not be loaded yet. The `boot` method is called after *all* other service providers have been registered, meaning you can safely access any registered service, route, or event listener.

### Q4. What are Laravel Facades, and how do they work?
* **Answer:** Facades provide a static "facade" interface to classes that are available inside the application's service container. Under the hood, they use dynamic static magic methods (`__callStatic()`) to forward calls from the facade to the underlying object bound in the container.

### Q5. Are Laravel Facades built on the standard Facade design pattern?
* **Answer:** No, they are inspired by it, but Laravel's facades act more like static proxies to dependency-injection containers rather than adhering strictly to the GoF facade design pattern.

### Q6. What are Laravel Contracts?
* **Answer:** Contracts are sets of interfaces that define core services provided by the framework (e.g., `Illuminate\Contracts\Queue\Queue`). They allow you to decouple your code and type-hint against interfaces instead of concrete classes, aiding testability.

### Q7. How does Laravel handle bootstrapping configuration?
* **Answer:** Configuration files are stored in the `config/` directory. They are loaded early in the request lifecycle and cached in production using the `php artisan config:cache` command to improve performance.

### Q8. What is the difference between `composer install` and `composer update`?
* **Answer:** `composer install` installs the exact dependency versions locked in `composer.lock`. `composer update` evaluates your version constraints in `composer.json`, updates packages to their latest matching versions, and rewrites the `composer.lock` file.

### Q9. What does the `php artisan optimize` command do?
* **Answer:** It caches your configuration, routes, and views into single files to reduce framework overhead during runtime request processing.

### Q10. What is a Single Action Controller?
* **Answer:** A controller that handles only one specific action, using a single `__invoke()` method. You define routes to it simply by passing the controller class name without specifying a method string.

---

## 2. Routing, Middleware & Controllers

### Q11. What is Middleware in Laravel?
* **Answer:** Middleware provides a mechanism for inspecting, filtering, or modifying HTTP requests entering your application (e.g., handling authentication, CORS, or maintenance mode).

### Q12. What is Route Model Binding?
* **Answer:** Route Model Binding automatically injects model instances directly into your routes or controllers based on route parameters. For example, matching `{user}` in a route automatically queries and injects the `User` model corresponding to that ID.

### Q13. How do you customize the key used in Route Model Binding (e.g., using a slug instead of an ID)?
* **Answer:** You can customize it directly in your route parameter definition by specifying the database column name after a colon: e.g., `/posts/{post:slug}`. Alternatively, override `getRouteKeyName()` on your Eloquent model.

### Q14. What is Reverse Routing?
* **Answer:** Reverse routing allows you to generate URLs or redirects using route names rather than hardcoding URI strings, ensuring links don't break if URI structures change.

### Q15. How do you exclude a URI from CSRF protection?
* **Answer:** You can exclude URIs by passing them to the `$except` array inside your `bootstrap/app.php` file (or legacy `VerifyCsrfToken` middleware), typically used for webhooks or specific API endpoints.

### Q16. What is the purpose of Form Request validation classes?
* **Answer:** Form Requests separate validation and authorization logic away from controllers. They encapsulate rules inside a dedicated class (`php artisan make:request`) and automatically execute before the controller method is even hit.

### Q17. How do you implement API Rate Limiting in Laravel?
* **Answer:** Laravel uses the `RateLimiter` facade inside a rate limiter configuration (often defined in `AppServiceProvider`). You can then apply it to routes using the `throttle` middleware (e.g., `->middleware('throttle:api')`).

### Q18. What is a Fallback Route?
* **Answer:** A fallback route (`Route::fallback()`) defines code that executes when no other route matches the incoming request URL, typically used to render a custom 404 error page.

### Q19. How do you create and register a custom macro?
* **Answer:** Macros allow you to add custom methods to internal Laravel core components (like collections, requests, or responses) at runtime. You define them using `Macroable::macro('name', function () { ... })`, typically inside a service provider's `boot` method.

### Q20. What is Route Caching and when should it be used?
* **Answer:** Route caching serializes your entire route collection into a single file using `php artisan route:cache`. It significantly boosts routing speed for production applications, but it will fail if your routes use closure-based routing.

---

## 3. Eloquent ORM & Databases

### Q21. What is Eloquent ORM?
* **Answer:** Eloquent is Laravel's built-in implementation of the Active Record pattern. It provides an object-oriented syntax where each database table has a corresponding Model used to interact with data.

### Q22. What is the N+1 Query Problem, and how do you fix it?
* **Answer:** The N+1 problem occurs when an initial query fetches a collection of records, and subsequent loop iterations trigger 1 additional query per record to fetch related data (totalling N+1 queries). It is fixed using **Eager Loading** via `with()` or **Lazy Eager Loading** via `load()`.

### Q23. What is the difference between `get()`, `first()`, and `find()`?
* **Answer:** `get()` returns a collection of multiple records; `first()` returns a single model instance representing the first matching record; `find()` looks up and returns a single record directly by its primary key.

### Q24. What are Accessors and Mutators?
* **Answer:** **Accessors** format or modify attributes when they are retrieved from the database. **Mutators** transform or clean data right before it is saved into the database. Modern Laravel uses the `Attribute::make()` definition method.

### Q25. What is the difference between Global Scopes and Local Scopes?
* **Answer:** **Local scopes** allow you to define common sets of query constraints that you can easily chain onto queries dynamically (e.g., `scopeActive($query)`). **Global scopes** automatically apply constraints to all queries executed for a given model (e.g., soft deletes or tenant scoping).

### Q26. What are Eloquent Relationships, and what are the main types?
* **Answer:** Relationships define associations between database tables. Main types include: One-to-One (`hasOne` / `belongsTo`), One-to-Many (`hasMany` / `belongsTo`), Many-to-Many (`belongsToMany` via pivot table), and Has-Many-Through / Has-One-Through.

### Q27. What are Polymorphic Relationships?
* **Answer:** Polymorphic relationships allow a model to belong to more than one type of model using a single association. For example, a `Comment` model could belong to both a `Post` and a `Video` model.

### Q28. How do you handle massive datasets efficiently using Eloquent?
* **Answer:** You can use `chunk()` to process records in small batches, `cursor()` to stream records via a generator using a single memory-efficient cursor, or `lazy()` to combine chunking with lazy collection iteration.

### Q29. What are Database Transactions, and how are they used in Laravel?
* **Answer:** Transactions ensure that a set of database operations succeed entirely or fail together atomically. Laravel handles this via `DB::transaction(function () { ... })` or manual `DB::beginTransaction()`, `commit()`, and `rollBack()` handling.

### Q30. What is Soft Deleting?
* **Answer:** Soft deleting flags a record as deleted by populating a `deleted_at` timestamp column instead of permanently removing the row from the database table. Eloquent automatically filters out soft-deleted records from standard queries.

---

## 4. Migrations, Seeders & Factories

### Q31. What are Database Migrations?
* **Answer:** Migrations act as version control for your database, allowing teams to modify, share, and synchronize database table schemas seamlessly.

### Q32. What is the difference between `migrate:fresh` and `migrate:refresh`?
* **Answer:** `migrate:refresh` rolls back all current migrations and re-runs them. `migrate:fresh` drops *all* database tables completely, regardless of migration files, and then runs all migrations from scratch.

### Q33. What are Database Seeders?
* **Answer:** Seeders are classes (`php artisan make:seeder`) containing logic to populate your database tables with dummy or initial default data using query builders or factories.

### Q34. What are Model Factories?
* **Answer:** Factories define blueprints to generate fake test model attributes using the Faker library, helping you test applications or seed development databases quickly.

### Q35. How do you seed relationships using factories?
* **Answer:** You can use factory state methods or relation definitions (e.g., `User::factory()->has(Post::factory()->count(3))->create()`).

### Q36. Can migrations be tracked to see which ones have executed?
* **Answer:** Yes, Laravel tracks executed migration batches using a database table named `migrations`.

### Q37. What is Migration Squashing?
* **Answer:** Migration squashing compresses dozens of sequential migration files into a single massive SQL schema schema-dump file to declutter your database migration directory.

### Q38. How do you write raw SQL inside a migration?
* **Answer:** You can execute arbitrary raw SQL queries within a migration file using the `DB::statement('sql query here')` method.

### Q39. What is schema builder in Laravel?
* **Answer:** The Schema builder (`Schema::create()`, `Schema::table()`) provides a database-agnostic programmatic interface to create and modify tables without writing raw SQL.

### Q40. How do you handle foreign key constraints inside migrations?
* **Answer:** You can define constraints using `$table->foreignId('user_id')->constrained()->onDelete('cascade');`, ensuring relational integrity on database levels.

---

## 5. Queues, Jobs & Events

### Q41. What are Queues in Laravel?
* **Answer:** Queues allow you to defer time-consuming tasks (such as sending emails or processing API calls) to be executed asynchronously in the background, improving HTTP request response times.

### Q42. What is the difference between a Job and an Event?
* **Answer:** An **Event** represents something that *has happened* in your system (e.g., `UserRegistered`), which can notify multiple listeners. A **Job** represents a specific instruction or task that *needs to be performed* (e.g., `SendWelcomeEmailJob`), which is typically pushed to a queue.

### Q43. What is the difference between `queue:work` and `queue:listen`?
* **Answer:** `queue:work` starts a persistent queue worker daemon that loads the framework once and stays in memory, making it extremely fast. `queue:listen` boots up a fresh framework instance for every single job, making it slower but ideal for local development because code changes take effect instantly without restarting.

### Q44. How do you handle failed background jobs?
* **Answer:** Failed jobs are caught and logged to a `failed_jobs` database table if configured. You can inspect them using `php artisan queue:failed` and retry them using `php artisan queue:retry {id}`.

### Q45. What is the difference between `Queue::push()` and `Queue::later()`?
* **Answer:** `Queue::push()` dispatches a job to be processed immediately by the next available worker. `Queue::later()` delays job execution for a specified duration or timestamp.

### Q46. What are Queue Batches?
* **Answer:** Queue batches allow you to execute a group of jobs simultaneously and run callback code (then/catch/finally) once the entire batch finishes or encounters a failure.

### Q47. What is Task Scheduling in Laravel?
* **Answer:** Task scheduling replaces complex cron-job configurations on your server with an expressive, centralized PHP schedule definition inside `routes/console.php` or `app/Console/Kernel.php`, driven by a single system cron entry running `php artisan schedule:run`.

### Q48. What are Model Observers?
* **Answer:** Observers group event-handling logic for Eloquent models (such as `creating`, `created`, `updating`, `deleted`), helping you keep models thin and decoupled.

### Q49. What is Event Broadcasting?
* **Answer:** Event broadcasting allows you to share server-side Laravel events with your client-side JavaScript application using WebSockets (via Laravel Reverb, Pusher, or Ably), enabling real-time UI updates.

### Q50. How do you chain queued jobs together?
* **Answer:** You can execute dependent background tasks sequentially using `Bus::chain([new JobOne(), new JobTwo()])->dispatch();`.

---

## 6. Security, Authentication & Authorization

### Q51. What is the difference between Authentication and Authorization?
* **Answer:** **Authentication** verifies *who you are* (logging in via credentials). **Authorization** determines *what you are allowed to do* (permissions and access control).

### Q52. What is CSRF (Cross-Site Request Forgery)?
* **Answer:** An attack that forces an authenticated user to execute unwanted actions on a web application in which they're currently logged in. Laravel mitigates this automatically by generating and validating a secret CSRF token for each active session.

### Q53. What is Mass Assignment Vulnerability?
* **Answer:** A security flaw where malicious users inject unexpected HTTP request parameters that map directly to protected database columns (e.g., passing `is_admin=1`). Laravel prevents this by requiring explicit `$fillable` or `$guarded` properties on models.

### Q54. What is SQL Injection, and how does Laravel prevent it?
* **Answer:** SQL injection happens when raw user input is concatenated directly into SQL execution strings. Laravel prevents this by default through Eloquent and the Query Builder, which use **PDO Parameterized Prepared Statements** under the hood.

### Q55. How should passwords be hashed in Laravel?
* **Answer:** Passwords should always be hashed using Laravel's `Hash` facade (`Hash::make('password')`), which utilizes secure underlying algorithms like Bcrypt or Argon2.

### Q56. What is the difference between Gates and Policies?
* **Answer:** **Gates** are simple, closure-based authorization checks typically used for general application actions not tied to specific models. **Policies** are dedicated classes that organize authorization logic around a specific model type (e.g., `PostPolicy` mapping to `Post` actions like update or delete).

### Q57. What are Laravel Guards?
* **Answer:** Guards define how users are authenticated for each request. Laravel ships with session guards (for traditional web apps) and token/Sanctum/Passport guards (for stateless APIs).

### Q58. What is Laravel Sanctum?
* **Answer:** Sanctum is a lightweight authentication system for SPAs (Single Page Applications), mobile applications, and simple token-based APIs, allowing issue-based API tokens and cookie-based session authentication.

### Q59. What is Laravel Passport?
* **Answer:** Passport is a full OAuth2 server implementation package for Laravel, providing robust token generation scopes, personal access tokens, and authorization code grants for complex enterprise APIs.

### Q60. How do you prevent XSS (Cross-Site Scripting) in Blade templates?
* **Answer:** Blade's standard double-curly syntax `{{ $variable }}` automatically passes output through PHP's `htmlspecialchars()` function to neutralize scripts. To render raw HTML intentionally, you must use `{!! $variable !!}`.

---

## 7. Caching, Performance & Optimization

### Q61. What is OPcache, and how does it help PHP performance?
* **Answer:** OPcache stores precompiled script bytecode in shared memory, preventing PHP from parsing, compiling, and translating script files on every single incoming HTTP request.

### Q62. What caching drivers does Laravel support out of the box?
* **Answer:** File, Database, Redis, Memcached, Array, and DynamoDB.

### Q63. How do you implement cache tagging?
* **Answer:** Cache tagging (supported by drivers like Redis and Memcached) allows you to group related cache items under tags so you can clear groups of items simultaneously using `Cache::tags(['posts', 'authors'])->flush();`.

### Q64. How do you optimize database query performance in Laravel?
* **Answer:** By adding proper database indexes, eager loading relationships to prevent N+1 queries, selecting only necessary columns (`select()`), and utilizing query result caching.

### Q65. What is HTTP Caching via Response headers?
* **Answer:** Laravel allows you response caching optimizations using middleware or conditional responses (`response()->noCache()`, `etag()`, `lastModified()`) to instruct browsers to cache static asset responses.

### Q66. How does Database Indexing speed up queries?
* **Answer:** Indexes create optimized B-tree data structures on table columns, allowing the database engine to locate records without performing slow full-table scans.

### Q67. What is the difference between Optimistic and Pessimistic Locking?
* **Answer:** **Pessimistic locking** locks database rows for reading/writing using `lockForUpdate()` during a transaction to prevent concurrent modifications. **Optimistic locking** assumes conflicts are rare, tracking row versions and failing updates if the record changed since it was read.

### Q68. How do you optimize Composer's autoloader for production deployment?
* **Answer:** By running `composer install --optimize-autoloader --no-dev`, which converts PSR-4 autoloading rules into a single map class lookup for faster file loading.

### Q69. What is view caching in Laravel?
* **Answer:** Blade template files are compiled down into raw PHP view files and stored on disk. Laravel serves these compiled cached files directly until the source Blade file changes.

### Q70. How do you handle configuration caching safely during deployment scripts?
* **Answer:** Run `php artisan config:cache` during deployment to compile all configuration files into a single array, but remember that calling `env()` inside configuration files will return `null` once cached (you must use `config()` helpers instead).

---

## 8. Testing & Debugging

### Q71. What is the difference between Unit Tests and Feature Tests?
* **Answer:** **Unit tests** focus on isolated, small pieces of code (like a single method or class helper) with minimal dependencies. **Feature tests** test larger code blocks and user flows, interacting with HTTP routes, database states, sessions, and middleware integration.

### Q72. How do you reset the database state between test runs?
* **Answer:** By using the `RefreshDatabase` trait on your test classes, which migrates your test database before the test suite runs and wraps each test in a database transaction to roll back changes automatically.

### Q73. How do you mock facades or services in Laravel tests?
* **Answer:** You can use Laravel's built-in testing helpers such as `Queue::fake()`, `Event::fake()`, `Notification::fake()`, or `Http::fake()` to intercept and assert behavior without executing real side-effects.

### Q74. How do you test HTTP endpoints in Laravel?
* **Answer:** By using the `$this->get('/api/users')` or `$this->postJson(...)` testing helpers, which return a `TestResponse` allowing assertions like `->assertStatus(200)` or `->assertJsonStructure(...)`.

### Q75. What is Laravel Dusk?
* **Answer:** Dusk is an expressive, automated browser testing and screen-scraping tool that runs headless Chrome instances to test JavaScript-heavy frontend interactions.

### Q76. How do you debug slow queries in Laravel?
* **Answer:** You can use `DB::listen(function ($query) { logger($query->sql, $query->bindings); })` to log raw queries, or install packages like Laravel Debugbar or Telescope.

### Q77. What is Laravel Telescope?
* **Answer:** Telescope is an elegant debug assistant panel package for local Laravel development, providing insight into incoming requests, exceptions, log entries, database queries, queued jobs, and mail.

### Q78. How do you handle exceptions globally in Laravel?
* **Answer:** You customize exception rendering and reporting behavior inside `bootstrap/app.php` (or legacy `app/Exceptions/Handler.php`) by chaining custom exception renderers using `withExceptions()`.

### Q79. What is Xdebug, and how is it used with Laravel?
* **Answer:** Xdebug is a PHP extension that communicates with IDEs (like PhpStorm or VS Code) to allow step-by-step debugging, breakpoint execution, and code-coverage analysis.

### Q80. How do you write data expectations using Pest PHP?
* **Answer:** Pest is an elegant testing framework built on top of PHPUnit that provides a clean, expressive syntax (e.g., `expect($user->name)->toBe('John');`).

---

## 9. APIs, Frontend & Services

### Q81. How does Laravel handle API versioning?
* **Answer:** Laravel doesn't have built-in API versioning, but it is typically handled by prefixing routes (e.g., `Route::prefix('v1')`) and creating separate controller namespaces for each version.

### Q82. What is an API Resource?
* **Answer:** API Resources act as a transformation layer between your Eloquent models and the JSON responses returned by your API, allowing you to format fields, hide sensitive attributes, and structure relationships consistently.

### Q83. What is Laravel Breeze vs. Jetstream?
* **Answer:** **Breeze** is a minimal, simple starting scaffolding package for authentication (Blade/Tailwind or Vue/React). **Jetstream** is a more robust application skeleton providing advanced features like two-factor authentication, team management, browser sessions, and API tokens.

### Q84. What is Livewire?
* **Answer:** Laravel Livewire is a full-stack framework for Laravel that makes building dynamic UIs simple using plain PHP, without leaving the comfort of Blade templates or writing heavy JavaScript frameworks.

### Q85. What is Inertia.js?
* **Answer:** Inertia acts as a glue between Laravel and modern frontend single-page frameworks (Vue, React, Svelte), allowing you to build SPAs using classic server-side routing without complex API client state management.

### Q86. What is Laravel Echo?
* **Answer:** A JavaScript library that makes it painless to subscribe to channels and listen for real-time WebSocket events broadcasted by your Laravel server.

### Q87. How do you consume external APIs in Laravel?
* **Answer:** Laravel provides an expressive, fluent HTTP wrapper client around Guzzle via the `Http` facade (e.g., `Http::withToken('token')->get(...)`).

### Q88. How do you handle file uploads securely in Laravel?
* **Answer:** Access uploaded files via the request object (`$request->file('avatar')`), store them securely using storage disks (e.g., `$path = $request->file('avatar')->store('avatars', 'public')`), validate mime types/extensions, and ensure files are not executable.

### Q89. What is Laravel Localization?
* **Answer:** Localization handles multi-language support, allowing you to translate text strings using translation files located in the `lang/` directory via the `__('messages.welcome')` helper function.

### Q90. What is Laravel Pennant?
* **Answer:** Pennant is an official feature-flag package for Laravel that lets you manage, track, and roll out new application features dynamically to users.

---

## 10. Advanced Architecture & Scaling

### Q91. How would you design a Multi-Tenant architecture in Laravel?
* **Answer:** Multi-tenancy can be implemented using three primary approaches: 1. Single Database with Tenant ID column (global scope filtering). 2. Database per Tenant (dynamically switching database connections per request). 3. Schema per Tenant (using separate PostgreSQL schemas).

### Q92. What are Custom Artisan Commands, and how do you create them?
* **Answer:** Custom console commands automate repetitive CLI tasks. You create them using `php artisan make:command Name`, define the signature and description, and write your logic inside the `handle()` method.

### Q93. What is Laravel Forge?
* **Answer:** Forge is a server management and provisioning tool created by Laravel that makes it easy to provision, configure, and deploy PHP applications on cloud providers like AWS, DigitalOcean, or Linode.

### Q94. What is Laravel Envoyer?
* **Answer:** Envoyer is a zero-downtime deployment tool for PHP applications, allowing you to deploy new Git releases seamlessly without dropping active user connections.

### Q95. How do you scale a high-traffic Laravel application across multiple servers?
* **Answer:** Use Load Balancers (Nginx/HAProxy) to distribute traffic, centralize Sessions and Caches using Redis, offload Queues to dedicated background worker servers, use a managed Database cluster with read-replicas, and enable OPcache.

### Q96. How do you handle Database Read Replicas in Laravel?
* **Answer:** Laravel supports automatic read/write database connections out of the box. You configure them in `config/database.php` under the `read` and `write` array keys, and Eloquent will route write operations to the primary database and reads to the replicas automatically.

### Q97. What is Laravel Pulse?
* **Answer:** Pulse is an official, real-time application performance monitoring and diagnostics dashboard package for Laravel, giving insights into slow requests, slow database queries, slow jobs, and server health.

### Q98. What is the Laravel Service Layer pattern, and why is it used?
* **Answer:** The Service Layer pattern extracts complex business logic out of controllers and models into dedicated service classes. This keeps controllers "skinny", improves testability, and promotes code reusability across web requests, CLI commands, and API endpoints.

### Q99. How do you manage dynamic configuration settings in a database instead of `env` files?
* **Answer:** Environment files (`.env`) should only hold deployment-specific environment variables. For runtime dynamic settings, you store them in a database table wrapped behind a cached repository service.

### Q100. When auditing a large legacy Laravel codebase, what are your first steps?
* **Answer:** 1. Check framework/dependency versions (`composer.json`). 2. Review application logging/error configs. 3. Scan for N+1 query patterns and missing database indexes. 4. Inspect controllers for fat logic bloat. 5. Check security headers, middleware rules, and mass-assignment protection on models.
