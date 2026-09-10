---
description: Assert against deeply nested data structures by path
---

# Data Navigator Expectations

Asserting against a deeply nested structure usually means a chain of intermediate variables and `structKeyExists()` guards before you reach the value you actually care about. The data navigator matchers collapse that into a single path expression, built on BoxLang's `dataNavigate()` built-in.

Paths support dot-notation, array indexes, wildcards, filters, recursive descent and the rest of the JSONPath-style expression set.

{% hint style="info" %}
These matchers require BoxLang. On CFML engines they throw `TestBox.BoxLangFeatureNotAvailable`.
{% endhint %}

The examples below all assume this structure:

```javascript
var data = {
    "app"   : { "name" : "TestApp", "settings" : { "debug" : true, "port" : 8080 } },
    "users" : [ { "name" : "Alice", "age" : 30 } ]
}
```

## Existence

### `toHavePath()`

Asserts a path resolves.

```javascript
expect( data ).toHavePath( "app.name" );
expect( data ).toHavePath( "app.settings.debug" );
expect( data ).toHavePath( "users[1].name" );

expect( data ).notToHavePath( "nonexistent" );
expect( data ).notToHavePath( "app.nonexistent" );
```

## Values

### `toHavePathValue()`

Asserts the value at a path equals an expected value.

```javascript
expect( data ).toHavePathValue( "app.name", "TestApp" );
expect( data ).toHavePathValue( "app.settings.port", 8080 );
expect( data ).toHavePathValue( "users[1].name", "Alice" );

expect( data ).notToHavePathValue( "app.name", "WrongApp" );
```

### `toHavePathType()`

Asserts the type at a path. Accepts standard types (`string`, `numeric`, `boolean`, `struct`, `array`) and the aliases `str`, `num`, `bool`, `arr`, `obj` and `map`.

```javascript
expect( data ).toHavePathType( "app.name", "string" );
expect( data ).toHavePathType( "app.settings.port", "numeric" );
expect( data ).toHavePathType( "app.settings", "struct" );
expect( data ).toHavePathType( "users", "array" );
expect( data ).toHavePathType( "app.settings.port", "num" );   // alias

expect( data ).notToHavePathType( "app.name", "numeric" );
```

### `toHavePathSatisfying()`

Asserts the value at a path satisfies a predicate closure, for anything equality cannot express.

```javascript
expect( data ).toHavePathSatisfying( "app.settings.port", port -> port > 1000 );
expect( data ).toHavePathSatisfying( "app.settings", s -> s.keyExists( "debug" ) );

expect( data ).notToHavePathSatisfying( "app.name", name -> name == "WrongName" );
```

## Chaining

### `path()`

Navigates to a path and hands back a normal `Expectation` on the value there, so you can chain any matcher you like rather than being limited to the path matchers above.

```javascript
expect( data ).path( "app.name" ).toBe( "TestApp" );
expect( data ).path( "app.settings.port" ).toBeGT( 8000 );
expect( data ).path( "users" ).toHaveLength( 1 );
expect( data ).path( "nonexistent" ).toBeNull();
```

### `queryPath()`

Navigates to a path and hands back an `Expectation` on an **array of every match**. This is the one to use with wildcards, filters and recursive descent, where a path resolves to many values rather than one.

```javascript
expect( data ).queryPath( "users[*].name" ).toHaveLength( 1 );
expect( data ).queryPath( "users[*].name" ).toInclude( "Alice" );
expect( data ).queryPath( "nonexistent" ).toBeEmpty();
```

## A Real Example

Validating an API response is where this earns its keep:

```javascript
it( "returns a well-formed user payload", function(){
    var response = api.get( "/users" );

    expect( response ).toHavePathValue( "status", 200 );
    expect( response ).toHavePathType( "data.users", "array" );

    // every user has an email, nobody leaks a password
    expect( response ).queryPath( "data.users[*].email" ).notToBeEmpty();
    expect( response ).notToHavePath( "data.users[*].password" );

    // pagination is sane
    expect( response ).toHavePathSatisfying( "meta.total", t -> t >= 0 );
    expect( response ).path( "meta.page" ).toBeGTE( 1 );
} );
```
