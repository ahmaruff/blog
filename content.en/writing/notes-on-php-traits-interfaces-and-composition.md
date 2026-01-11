+++
date = '2025-06-23T20:23:24+07:00'
draft = false
menus = 'writing'
title = 'Notes on PHP: Traits, Interfaces, and Class Composition'
tags = ["php", "laravel"]
+++

Today I learned a few PHP features that I had never really thought about before.
Thanks to Laravel, most of these things are already abstracted away, so I rarely think about PHP deeply as a programming language itself.

### 1. Interface

Just like in other languages, an interface is commonly used as a contract for a class that can have multiple implementations.
For example, an `UserRepository` interface can be implemented using MySQL or PostgreSQL.

This allows both implementations to share the same method signatures, so any consumer of the interface doesn’t need to change anything when the underlying implementation changes.

Example:

```php
interface UserRepository
{
    public function set($params): string;
    public function get(): string;
}

class MysqlUserRepository implements UserRepository
{
    public function set($params): string
    {
        // MySQL implementation
    }

    public function get(): string
    {
        // implementation
    }
}

class PgUserRepository implements UserRepository
{
    public function set($params): string
    {
        // PostgreSQL implementation
    }

    public function get(): string
    {
        // implementation
    }
}
```
Both classes implement the same interface, so any code that depends on `UserRepository` can use its methods without knowing the concrete implementation.

### 2. Trait

A trait is basically a collection of methods that can be composed into multiple classes.
It’s usually used to share common functionality across classes without using inheritance.

Example:

```php
trait HasSlug
{
    public function generateSlug(string $title): string
    {
        return strtolower(str_replace(' ', '-', $title));
    }
}

class Blog
{
    use HasSlug; // Blog automatically has generateSlug()
}
```

### 3. Favor Composition Over Inheritance

In general, we are encouraged to use composition (for example via dependency injection) instead of inheritance.
Composition is more flexible, easier to test, and less tightly coupled.

Example:

```php
class User
{
    protected $name;

    public function name(): string
    {
        return $this->name;
    }
}

class Blog
{
    public function __construct(public User $user) {}

    public function name(): string
    {
        return $this->user->name();
    }
}

```

In this example, we can access `User.name` without using inheritance.
This becomes even more powerful if the constructor accepts an interface instead of a concrete class,
because it makes it easier to swap implementations later.

PHP used to have a bad reputation — messy and painful to work with.
And honestly, that wasn’t completely wrong back then.

But after understanding these “modern language” features, I realized PHP can actually be clean and elegant.
Laravel hides a lot of complexity, but knowing the fundamentals makes coding much more enjoyable.
