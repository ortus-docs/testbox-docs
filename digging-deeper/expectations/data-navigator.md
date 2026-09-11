---
description: Assert against deeply nested data structures by path
---

# Data Navigator Expectations

Asserting against a deeply nested structure usually means a chain of intermediate variables and `structKeyExists()` guards before you reach the value you actually care about. The data navigator matchers collapse that into a single path expression, built on BoxLang's `dataNavigate()` built-in.

Paths are JSONPath-style expressions, supporting dot notation, array indexing, recursive descent, wildcards, slices and filters. The full syntax is listed under [Path Syntax](#path-syntax) below.

{% hint style="info" %}
These matchers require BoxLang. On CFML engines they throw `TestBox.BoxLangFeatureNotAvailable`.
{% endhint %}

The examples below all assume this structure:

```java
var data = {
    "app"   : { "name" : "TestApp", "settings" : { "debug" : true, "port" : 8080 } },
    "users" : [ { "name" : "Alice", "age" : 30 } ]
}
```

## Path Syntax

Every matcher on this page takes the same path expression, handed straight to BoxLang's `dataNavigate()`.

| Syntax | Example | Matches |
| --- | --- | --- |
| Dot notation | `app.settings.port` | The value at that key path |
| Array index | `users[1]` | The **first** user — indexing is 1-based |
| Recursive descent | `..name` | Every `name` key at any depth |
| Wildcard, keys | `app.settings.*` | Every value under `settings` |
| Wildcard, array | `users[*].name` | The `name` of every user |
| Slice | `primes[1:3]` | Elements 1 through 3, inclusive |
| Open-ended slice | `primes[2:]` | Element 2 through the end |
| Filter | `users[?(@.age > 18)].name` | The `name` of every user over 18 |

{% hint style="info" %}
Array indexing is **1-based**, following the language convention rather than JSONPath's usual 0-based indexing. `users[1]` is the first element, not the second.
{% endhint %}

Filters use `@` to refer to the current element and support `==`, `!=`, `>`, `<`, `>=` and `<=`. Whitespace inside the filter is tolerated, so `[?(@.active==true)]` and `[?( @.active == true )]` are equivalent.

{% hint style="success" %}
A path that resolves to many values — anything using a wildcard, slice, recursive descent or filter — is best paired with `queryPath()`, which returns every match. `path()` and the `toHavePath*()` matchers resolve to the first match.
{% endhint %}

## Existence

### `toHavePath()`

Asserts a path resolves.

```java
expect( data ).toHavePath( "app.name" )
expect( data ).toHavePath( "app.settings.debug" )
expect( data ).toHavePath( "users[1].name" )

expect( data ).notToHavePath( "nonexistent" )
expect( data ).notToHavePath( "app.nonexistent" )
```

## Values

### `toHavePathValue()`

Asserts the value at a path equals an expected value.

```java
expect( data ).toHavePathValue( "app.name", "TestApp" )
expect( data ).toHavePathValue( "app.settings.port", 8080 )
expect( data ).toHavePathValue( "users[1].name", "Alice" )

expect( data ).notToHavePathValue( "app.name", "WrongApp" )
```

### `toHavePathType()`

Asserts the type at a path. Accepts standard types (`string`, `numeric`, `boolean`, `struct`, `array`) and the aliases `str`, `num`, `bool`, `arr`, `obj` and `map`.

```java
expect( data ).toHavePathType( "app.name", "string" )
expect( data ).toHavePathType( "app.settings.port", "numeric" )
expect( data ).toHavePathType( "app.settings", "struct" )
expect( data ).toHavePathType( "users", "array" )
expect( data ).toHavePathType( "app.settings.port", "num" );   // alias

expect( data ).notToHavePathType( "app.name", "numeric" )
```

### `toHavePathSatisfying()`

Asserts the value at a path satisfies a predicate closure, for anything equality cannot express.

```java
expect( data ).toHavePathSatisfying( "app.settings.port", port -> port > 1000 )
expect( data ).toHavePathSatisfying( "app.settings", s -> s.keyExists( "debug" ) )

expect( data ).notToHavePathSatisfying( "app.name", name -> name == "WrongName" )
```

## Chaining

### `path()`

Navigates to a path and hands back a normal `Expectation` on the value there, so you can chain any matcher you like rather than being limited to the path matchers above.

```java
expect( data ).path( "app.name" ).toBe( "TestApp" )
expect( data ).path( "app.settings.port" ).toBeGT( 8000 )
expect( data ).path( "users" ).toHaveLength( 1 )
expect( data ).path( "nonexistent" ).toBeNull()
```

### `queryPath()`

Navigates to a path and hands back an `Expectation` on an **array of every match**. This is the one to use with wildcards, slices, filters and recursive descent, where a path resolves to many values rather than one.

```java
// wildcards
expect( data ).queryPath( "users[*].name" ).toHaveLength( 1 )
expect( data ).queryPath( "users[*].name" ).toInclude( "Alice" )

// filters: only the users who pass the predicate
expect( data ).queryPath( "users[?(@.age > 18)].name" ).toInclude( "Alice" )

// recursive descent: every matching key at any depth
expect( data ).queryPath( "..name" ).toInclude( "TestApp" )

expect( data ).queryPath( "nonexistent" ).toBeEmpty()
```

Slices take an inclusive, 1-based window into an array:

```java
var primes = { "values" : [ 2, 3, 5, 7, 11 ] }

expect( primes ).queryPath( "values[1:3]" ).toHaveLength( 3 )   // 2, 3, 5
expect( primes ).queryPath( "values[4:]" ).toHaveLength( 2 )    // 7, 11
```

## A Real Example

Validating an API response is where this earns its keep:

```java
it( "returns a well-formed user payload", function(){
    var response = api.get( "/users" )

    expect( response ).toHavePathValue( "status", 200 )
    expect( response ).toHavePathType( "data.users", "array" )

    // every user has an email, nobody leaks a password
    expect( response ).queryPath( "data.users[*].email" ).notToBeEmpty()
    expect( response ).notToHavePath( "data.users[*].password" )

    // pagination is sane
    expect( response ).toHavePathSatisfying( "meta.total", t -> t >= 0 )
    expect( response ).path( "meta.page" ).toBeGTE( 1 )
} )
```
