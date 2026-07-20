# PHP Interview Notebook

> **Mid to Senior Level Preparation - 100 Essential Questions & Answers**

## 1. Core PHP Fundamentals

### Q1. What is the difference between `==` and `===`?
**Answer:** The `==` operator compares values after type juggling (loose comparison). The `===` operator compares both value and data type strictly without type juggling.

### Q2. Explain `require`, `require_once`, `include`, and `include_once`.
**Answer:** `require` throws a Fatal Error if the file is missing, halting execution. `include` throws a Warning and continues execution. The `_once` variants ensure the file is loaded only once per request.

### Q3. What are Superglobals in PHP?
**Answer:** Superglobals are built-in variables available in all scopes throughout a script. Examples include `$_GET`, `$_POST`, `$_SESSION`, `$_COOKIE`, `$_SERVER`, and `$_ENV`.

### Q4. How do Sessions differ from Cookies?
**Answer:** Sessions store user data on the server with a unique session ID sent to the user via a cookie. Cookies store data directly on the user's browser, which can be modified by the user.

### Q5. What is the difference between `GET` and `POST`?
**Answer:** `GET` appends form data to the URL (visible, limited size, cached). `POST` sends data in the HTTP request body (hidden from URL, larger payloads, not cached).

### Q6. What is the `php.ini` file?
**Answer:** It is the default configuration file for PHP. It controls settings like memory limits, execution time, error reporting, and file upload limits.

### Q7. How do you enable error reporting in PHP?
**Answer:** In `php.ini`, set `display_errors = On` and `error_reporting = E_ALL`. At runtime, use `error_reporting(E_ALL); ini_set('display_errors', 1);`.

### Q8. What does the `isset()` function do?
**Answer:** It checks if a variable is declared and is different from `null`. It returns `true` if it exists and is not null.

### Q9. What is the difference between `isset()` and `empty()`?
**Answer:** `empty()` checks if a variable is empty (e.g., '', 0, '0', null, false, or an empty array). `isset()` only checks if it is declared and not null.

### Q10. What does `unset()` do?
**Answer:** It destroys a specified variable, removing it from memory.

### Q11. What is a closure in PHP?
**Answer:** A closure is an anonymous function that can capture variables from the outside scope using the `use` keyword.

### Q12. What is the difference between single quotes and double quotes for strings?
**Answer:** Double quotes parse variables (e.g., `"Hello $name"`) and escape sequences like `\n`. Single quotes treat content literally, making them slightly faster.

### Q13. How do you handle file uploads in PHP?
**Answer:** Uploaded files are accessed via the `$_FILES` superglobal. Use `move_uploaded_file()` to securely move the file from the temporary directory to a permanent location.

### Q14. What is the spaceship operator (`<=>`)?
**Answer:** Introduced in PHP 7, it compares two expressions and returns -1, 0, or 1 if the left is less than, equal to, or greater than the right.

### Q15. What is the null coalescing operator (`??`)?
**Answer:** It returns its first operand if it exists and is not null; otherwise, it returns its second operand. Example: `$name = $_GET['user'] ?? 'Guest';`.

### Q16. What are variable variables?
**Answer:** A variable whose name is evaluated dynamically. Example: `$a = 'hello'; $$a = 'world';` makes `$hello` equal to 'world'.

### Q17. What is `var_dump()` used for?
**Answer:** It displays structured information about one or more variables, including its type and value. Essential for debugging.

### Q18. How do you extend the execution time of a PHP script?
**Answer:** Using `set_time_limit(seconds)` in the script or altering `max_execution_time` in `php.ini`.

### Q19. What is a constant in PHP and how is it defined?
**Answer:** A constant is a simple value that cannot change during execution. Define it using `define('NAME', 'value')` or `const NAME = 'value';`.

### Q20. What is the purpose of the `header()` function?
**Answer:** It is used to send raw HTTP headers to a client, typically for redirects (`header('Location: url')`) or setting content types.

---

## 2. Arrays, Strings & Data Structures

### Q21. What is the difference between indexed and associative arrays?
**Answer:** Indexed arrays use numeric keys (0, 1, 2). Associative arrays use named string keys (e.g., `['name' => 'John']`).

### Q22. How does `array_map()` work?
**Answer:** It applies a callback function to each element of the given arrays and returns a new array with the modified values.

### Q23. What is the difference between `array_map()` and `array_filter()`?
**Answer:** `array_map()` modifies elements and returns an array of the same size. `array_filter()` filters out elements based on a callback's boolean return, potentially returning a smaller array.

### Q24. What does `array_reduce()` do?
**Answer:** It iteratively reduces an array to a single value using a callback function (e.g., summing all numbers in an array).

### Q25. How do you merge arrays in PHP?
**Answer:** Using `array_merge($arr1, $arr2)` (re-indexes numeric keys) or the `+` operator (`$arr1 + $arr2`, preserves keys but doesn't overwrite existing ones).

### Q26. Explain the `explode()` and `implode()` functions.
**Answer:** `explode()` splits a string into an array by a delimiter. `implode()` joins array elements into a string with a glue string.

### Q27. How do you check if a key exists in an array?
**Answer:** Use `array_key_exists('key', $array)` or `isset($array['key'])` (though `isset` returns false if the key exists but is null).

### Q28. What does `in_array()` do?
**Answer:** Checks if a specific value exists in an array. It takes a third parameter for strict (`===`) type checking.

### Q29. What is SPL in PHP?
**Answer:** Standard PHP Library (SPL) is a collection of interfaces and classes meant to solve standard problems (e.g., Iterators, Data Structures like Stacks and Queues).

### Q30. What are Generators in PHP?
**Answer:** Generators allow iterating through a large dataset without loading it all into memory. They use the `yield` keyword to return values one at a time.

### Q31. What is the benefit of `yield` over returning an array?
**Answer:** `yield` produces a Generator object, drastically reducing memory usage when processing thousands or millions of records.

### Q32. How do you find the length of a string?
**Answer:** Use `strlen()`. For multibyte characters (like UTF-8), use `mb_strlen()`.

### Q33. What is the difference between `strpos()` and `strstr()`?
**Answer:** `strpos()` returns the numeric position of the first occurrence of a substring. `strstr()` returns the portion of the string from that occurrence to the end.

### Q34. How do you replace text within a string?
**Answer:** Use `str_replace('search', 'replace', $subject)`. For regular expressions, use `preg_replace()`.

### Q35. What does `array_values()` do?
**Answer:** It returns all the values from an array and indexes the array numerically, useful for resetting array keys.

### Q36. What does `array_keys()` do?
**Answer:** It returns an array containing all the keys (numeric or string) of the given array.

### Q37. How do you sort an array by its values and keys?
**Answer:** `sort()` for values (loses keys), `asort()` for values (keeps keys), `ksort()` sorts by keys.

### Q38. What does `extract()` do?
**Answer:** It imports variables from an associative array into the current symbol table (e.g., `['a' => 1]` creates `$a = 1`). Can be a security risk with untrusted data.

### Q39. What is the difference between `json_encode()` and `json_decode()`?
**Answer:** `json_encode()` converts a PHP array/object to a JSON string. `json_decode()` converts a JSON string back into a PHP object (or associative array if the second parameter is true).

### Q40. What does the `list()` construct or `[]` array destructuring do?
**Answer:** It assigns variables as if they were an array. Example: `[$a, $b] = [1, 2];` assigns 1 to $a and 2 to $b.

---

## 3. Object-Oriented Programming (OOP)

### Q41. What are the four pillars of OOP?
**Answer:** Encapsulation, Abstraction, Inheritance, and Polymorphism.

### Q42. What is an Interface in PHP?
**Answer:** An interface defines a contract of methods a class must implement without providing the method bodies. A class can implement multiple interfaces.

### Q43. What is an Abstract Class?
**Answer:** A class that cannot be instantiated on its own. It can contain both abstract methods (to be implemented by children) and standard methods with logic.

### Q44. Interface vs. Abstract Class: When to use which?
**Answer:** Use an abstract class to share core logic between related classes. Use an interface to define a strict behavioral contract across unrelated classes.

### Q45. What are Traits?
**Answer:** Traits allow code reuse in single-inheritance languages like PHP. You can group methods in a Trait and `use` them in multiple independent classes.

### Q46. How do you resolve Trait conflicts?
**Answer:** If two traits have methods with the same name, use the `insteadof` operator to specify which method to use, and `as` to alias the other.

### Q47. What is Late Static Binding?
**Answer:** Using `static::` instead of `self::` to reference the class that was called at runtime, rather than the class where the method was defined.

### Q48. What are Magic Methods?
**Answer:** Methods starting with `__` that are triggered by specific actions. Examples: `__construct()`, `__get()`, `__set()`, `__call()`, `__toString()`.

### Q49. What is `__construct()`?
**Answer:** The constructor method automatically called when a new object instance is created.

### Q50. What is Dependency Injection?
**Answer:** Passing dependent objects into a class (usually via the constructor) rather than having the class instantiate them. It improves testability and loose coupling.

### Q51. What is polymorphism in PHP?
**Answer:** The ability of different classes to respond to the same method call in their own way, usually achieved by implementing the same interface.

### Q52. What is encapsulation?
**Answer:** Hiding the internal state of an object and requiring all interaction to be performed through an object's methods (using `public`, `protected`, `private` visibility).

### Q53. What is the difference between `public`, `protected`, and `private`?
**Answer:** `public`: accessible anywhere. `protected`: accessible within the class and extending classes. `private`: accessible only within the class that defines it.

### Q54. What does the `final` keyword do?
**Answer:** When applied to a class, it prevents the class from being extended. When applied to a method, it prevents the method from being overridden by a child class.

### Q55. What are Namespaces?
**Answer:** Namespaces isolate code into distinct areas to avoid class name collisions (e.g., `App\Models\User` vs `Vendor\Library\User`).

### Q56. What is PSR-4?
**Answer:** It is the standard for autoloading classes from file paths. It maps namespaces to directory structures, commonly used by Composer.

### Q57. What is Method Overriding?
**Answer:** When a child class redefines a method from its parent class with the exact same name and signature.

### Q58. Does PHP support Method Overloading?
**Answer:** Not in the traditional sense (multiple methods with the same name but different signatures). PHP simulates it using the magic method `__call()`.

### Q59. What is the Singleton Pattern?
**Answer:** A design pattern that ensures a class has only one instance and provides a global point of access to it. Often considered an anti-pattern today.

### Q60. What is the Factory Pattern?
**Answer:** A creational pattern that uses factory methods to deal with the problem of creating objects without specifying the exact class of object that will be created.

---

## 4. Modern PHP (PHP 7.x & 8.x Features)

### Q61. What does `declare(strict_types=1);` do?
**Answer:** It enforces strict type checking for function parameters and return types in the file it is declared, preventing automatic type juggling.

### Q62. What are Typed Properties?
**Answer:** Introduced in PHP 7.4, it allows specifying data types directly on class properties (e.g., `public string $name;`).

### Q63. What are Arrow Functions (`fn() =>`)?
**Answer:** Introduced in PHP 7.4, they provide a concise syntax for closures and automatically capture variables from the parent scope by value.

### Q64. What is Constructor Property Promotion? (PHP 8.0)
**Answer:** A shorthand to declare and initialize class properties directly in the constructor parameters (e.g., `public function __construct(public string $name) {}`).

### Q65. What is the Nullsafe Operator (`?->`)? (PHP 8.0)
**Answer:** It allows reading properties and calling methods on an object that might be null. If it is null, it aborts the chain and evaluates to null instead of throwing an error.

### Q66. What are Match Expressions? (PHP 8.0)
**Answer:** A strict, value-returning alternative to `switch`. It does not require `break` statements and uses strict comparison (`===`).

### Q67. What are Union Types? (PHP 8.0)
**Answer:** Allows a variable to accept multiple specific types, separated by a pipe (e.g., `public function set(int|float $value)`).

### Q68. What are Named Arguments? (PHP 8.0)
**Answer:** Allows passing arguments to a function based on the parameter name rather than position (e.g., `setcookie(name: 'test', value: '1');`).

### Q69. What are Enums? (PHP 8.1)
**Answer:** Enumerations restrict a variable to a predefined set of constant object values, providing much better type safety than basic strings or integers.

### Q70. What are Readonly Properties? (PHP 8.1)
**Answer:** Properties declared with `readonly` can only be initialized once. After that, they cannot be changed, ensuring immutability.

### Q71. What are Intersection Types? (PHP 8.1)
**Answer:** Requires a value to satisfy *all* specified types simultaneously, usually multiple interfaces (e.g., `Iterator&Countable`).

### Q72. What are Attributes (Annotations)? (PHP 8.0)
**Answer:** Structured, machine-readable metadata added to classes, methods, or properties using `#[AttributeName]` syntax.

### Q73. What is the `mixed` type? (PHP 8.0)
**Answer:** A pseudo-type that represents any type (array, bool, callable, int, float, null, object, resource, string).

### Q74. What is the `never` return type? (PHP 8.1)
**Answer:** Indicates that a function will not return a value and will either throw an exception or terminate execution (e.g., `exit()` or `die()`).

### Q75. What is a First-class Callable? (PHP 8.1)
**Answer:** A syntax (`$func(...)`) to capture a closure from a callable, making it easier to pass methods as callbacks.

### Q76. What is a Disjunctive Normal Form (DNF) Type? (PHP 8.2)
**Answer:** Allows combining Union and Intersection types (e.g., `(A&B)|C`).

### Q77. What are Readonly Classes? (PHP 8.2)
**Answer:** Marking a class as `readonly` implicitly marks all of its properties as `readonly` and prevents dynamic property creation.

### Q78. What is the `#[SensitiveParameter]` attribute? (PHP 8.2)
**Answer:** Hides the parameter's value in stack traces when an exception is thrown, useful for passwords or API keys.

### Q79. What are Property Hooks? (PHP 8.4+)
**Answer:** Allows defining `get` and `set` logic directly on a property, eliminating boilerplate getter/setter methods.

### Q80. What is the difference between `self`, `parent`, and `static` return types?
**Answer:** `self` returns the class where the method is defined. `parent` returns the parent class. `static` (PHP 8.0+) returns the actual class instantiated at runtime.

---

## 5. Architecture, Security, Performance & Databases

### Q81. How do you prevent SQL Injection?
**Answer:** Always use Prepared Statements with bound parameters via PDO or MySQLi. Never concatenate user input into SQL strings.

### Q82. What is PDO?
**Answer:** PHP Data Objects (PDO) is a database access layer providing a uniform method of access to multiple databases. It supports prepared statements.

### Q83. What is XSS and how do you prevent it?
**Answer:** Cross-Site Scripting occurs when untrusted user data is rendered in the browser. Prevent it by escaping output using `htmlspecialchars()`.

### Q84. What is CSRF and how do you prevent it?
**Answer:** Cross-Site Request Forgery tricks a user into submitting malicious requests. Prevent it by using anti-CSRF tokens in forms and verifying them on the server.

### Q85. How should passwords be stored in PHP?
**Answer:** Never store plain text or use MD5/SHA1. Use `password_hash()` with the `PASSWORD_BCRYPT` or `PASSWORD_ARGON2I` algorithm, and verify with `password_verify()`.

### Q86. What is the difference between Error and Exception in PHP 7+?
**Answer:** Both implement the `Throwable` interface. Exceptions are thrown intentionally, while Errors are thrown by PHP for internal system errors (like calling a non-existent method).

### Q87. What is OPcache?
**Answer:** A caching engine built into PHP that stores precompiled script bytecode in shared memory, removing the need for PHP to load and parse scripts on each request.

### Q88. How do you handle background jobs/heavy tasks?
**Answer:** PHP is synchronous. Heavy tasks should be offloaded to a message queue (like Redis/RabbitMQ) and processed by background CLI workers.

### Q89. What is the N+1 query problem?
**Answer:** An ORM performance issue where fetching N related records causes N additional queries instead of a single query. Solved by Eager Loading.

### Q90. What are SOLID principles?
**Answer:** Five design principles for OOP: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion.

### Q91. What is the purpose of Composer?
**Answer:** Composer is a dependency manager for PHP. It manages external libraries, resolves versions, and provides an autoloader for your project.

### Q92. What is a REST API?
**Answer:** An architectural style for APIs that uses standard HTTP methods (GET, POST, PUT, DELETE), stateless communication, and standard formats like JSON.

### Q93. What HTTP status code is used for 'Not Found' and 'Unauthorized'?
**Answer:** 404 for Not Found. 401 for Unauthorized (authentication required). 403 for Forbidden (authenticated but lacking permissions).

### Q94. How does PHP handle Garbage Collection?
**Answer:** PHP uses reference counting and a cycle-collecting algorithm to automatically free memory when variables are no longer referenced.

### Q95. What is the `phpinfo()` function used for?
**Answer:** It outputs extensive information about the current state of PHP, including configuration options, extensions, and server environment variables. (Keep it secure in production).

### Q96. How do you prevent Session Hijacking?
**Answer:** Regenerate the session ID after login using `session_regenerate_id()`, use `HttpOnly` and `Secure` flags on session cookies, and bind sessions to user agents/IPs cautiously.

### Q97. What is an active record vs data mapper pattern?
**Answer:** Active Record maps objects directly to database rows (like Eloquent). Data Mapper separates the in-memory objects from the database (like Doctrine).

### Q98. How do you scale a PHP application?
**Answer:** Use load balancers, centralize sessions (e.g., Redis), use OPcache, offload background jobs to queues, implement caching (Memcached/Redis), and optimize databases.

### Q99. What are PHP generators best used for?
**Answer:** Processing massive files (like gigabyte CSVs) or large database query result sets where storing the whole array in memory would hit `memory_limit`.

### Q100. Why is `eval()` considered dangerous?
**Answer:** It executes arbitrary strings as PHP code. If user input reaches `eval()`, an attacker can execute malicious scripts and compromise the entire server.

---

