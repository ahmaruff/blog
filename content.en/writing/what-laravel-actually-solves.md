+++
date = '2026-01-10T13:00:24+07:00'
draft = false
menus = 'writing'
title = 'Unpacking the "Magic" of Laravel: What Does It Actually Solve?'
tags = ["php", "laravel", "framework", "concepts"]
+++

When starting in web development, many of us jump straight into using a framework.
After getting acquainted with PHP, I started with CodeIgniter 4, and for the last two-plus years, I've been using Laravel professionally.

Laravel is an incredible tool for speeding up development.
However, this convenience often makes us complacent.
Honestly, I rarely thought deeply about what was happening behind the scenes.
We take the conveniences provided by the framework for granted.

To answer that curiosity, I conducted a simple experiment: I built two identical CRUD applications.

- **One version in Laravel.**
- **One version in Native PHP** (with no framework at all).

Both were simple book management applications using SQLite and an MVC architecture, with a UI powered by 98.css.

The goal was clear:
> To better understand the MVC concept and to see concretely what technical problems Laravel actually solves.

### Why Is This Experiment Important?

Frameworks simplify many things. But that convenience can be a double-edged sword.
We forget the actual request processing flow, no longer understand the application lifecycle, and let everything run like magic.

By building the native version, I was forced to:
- Write every component from scratch.
- Experience each architectural layer—from routing to the view—in a tangible way.
- Work without any hidden abstractions.

The result was a much clearer understanding of how web applications work. Let's break it down.

## 1. Routing: From Query Strings to Resource Controllers

**Native PHP**  
Here, routing is a manual job. I used a query string to determine which page to display.
```php
// URL: ?c=book&a=show&id=3
```
A front controller (`public/index.php`) is responsible for reading the `c` (controller) and `a` (action) parameters, then loading the appropriate file and calling the method.
The process is transparent but also repetitive and prone to errors.

**Laravel**  
A single, elegant line in `routes/web.php` is all it takes:
```php
Route::resource('books', BookController::class);
```
Laravel automatically handles everything: parsing the URI, matching the route with the HTTP method (GET, POST, PUT, DELETE), and validating parameters.

**What Laravel Solves:**
- URI parsing and parameter extraction.
- Complex route-matching logic.
- Dispatching the request to the correct controller method.

## 2. Controller: From Manual Loading to Dependency Injection

**Native PHP**  
I had to `require` each controller manually. Object instantiation was also done by hand, and then the method was called directly.
```php
$controller = new BookController();
$controller->show(3);
```

**Laravel**  
Thanks to Composer's autoloader and its service container, Laravel manages the entire object lifecycle.
Dependencies like the request object or other service classes can be automatically injected into methods.

**What Laravel Solves:**
- **IoC Container**: A smart container that manages object creation and provisioning.
- **Object Lifecycle**: Manages when objects are created and destroyed.
- **Dependency Resolution**: Automatically injects dependencies, leading to cleaner, more modular code.

## 3. Data Layer: From Manual PDO to the Eloquent ORM

**Native PHP**  
I had to create a PDO connection, write raw SQL queries, and manually map the results to an array or object.
This process is lengthy and vulnerable to SQL injection if not handled carefully.

**Laravel**  
With the Eloquent ORM, database operations become much simpler and more expressive.
```php
$books = Book::all();
$book = Book::find(1);
```
**What Laravel Solves:**
- **Query Builder & ORM**: Provides a safe and readable abstraction for database interaction.
- **Connection Management**: Manages database connections efficiently.
- **Object Hydration**: Automatically transforms query results into PHP objects.

## 4. View Rendering: From `include()` to the Blade Engine

**Native PHP**  
Displaying a view is done with a standard PHP `include`. Data must be passed manually, and the logic for layout templating has to be built from scratch.
The developer is also fully responsible for escaping output to prevent XSS attacks.

**Laravel**  
Blade, Laravel's built-in template engine, offers a clean syntax and powerful features like layout inheritance, a component system, and automatic output escaping.
```blade
@extends('layouts.app')

@section('content')
    <h1>{{ $book->title }}</h1>
@endsection
```
**What Laravel Solves:**
- Efficient template parsing.
- Secure output by default.
- View caching for better performance.
- A reusable component system.

## 5. Database Migration: From PHP Scripts to Versioning

**Native PHP**  
Database schema changes are managed with manual PHP or SQL scripts. There is no versioning system, which makes team collaboration and rollbacks extremely difficult.

**Laravel**  
Just one command in the terminal:
```bash
php artisan migrate
```
**What Laravel Solves:**
- **Version Control for Your Database**: Every schema change is versioned and trackable.
- **Rollbacks**: The ability to easily revert to a previous schema version.
- **Schema Abstraction**: Write schema definitions in PHP, which can be run against various database systems (MySQL, PostgreSQL, etc.).

## Final Thoughts

This experiment opened my eyes. Laravel isn't just a "get it done fast" framework.
It's the result of research and implementation of solutions to real, complex engineering problems.

But there's another crucial lesson: **without understanding the fundamentals, you're just a tool user, not a craftsman who has mastered it.**

Laravel doesn't replace a basic understanding of HTTP, MVC, or OOP.
On the contrary, that foundational knowledge is what allows us to use Laravel—and other frameworks—more wisely, efficiently, and creatively.

This small experiment has sharpened my understanding of the request lifecycle, given me more confidence in debugging,
and made me appreciate the design decisions behind Laravel even more.

---

### Source Code

For those interested in seeing the direct comparison, here are the repositories for both applications:

- **Laravel Version**: [emvici-laravel](https://github.com/ahmaruff/emvici-laravel)
- **Native PHP Version**: [emvici-php](https://github.com/ahmaruff/emvici-php)
