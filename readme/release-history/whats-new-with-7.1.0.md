---
description: September 10, 2026
---

# What's New With 7.1.0

TestBox 7.1.0 introduces a significant upgrade to the assertions and expectations library, adding grouped assertions, collection expectation modes, rich failure diagnostics, and a suite of new matchers inspired by JUnit 5 and Jasmine.

* * *

## Expectation Context: `withContext()`

Add semantic context to any expectation so failure messages include identifying information. Works with all matchers, negated matchers, and custom matchers.

```javascript
it( "validates a user record", () => {
    expect( user.age )
        .withContext( "user.age" )
        .toBeGT( 0 )

    expect( user.email )
        .withContext( "user.email" )
        .toMatch( "@" )
} )
```

* * *

## Collection Expectation Modes

Three new collection modes complement the existing `expectAll()`.

### `expectAny()`

Passes when at least one element in the collection passes the chained matcher.

```javascript
expectAny( products ).toSatisfy( p => p.onSale )
```

### `expectSome()`

Passes when a bounded number of elements pass. `max = 0` means no upper bound.

```javascript
expectSome( users, min = 2, max = 5 ).toSatisfy( u => u.role == "admin" )
expectSome( items, min = 3 ).toSatisfy( i => i.stock > 0 )
```

### `expectNone()`

Passes when zero elements pass the chained matcher.

```javascript
expectNone( users ).toSatisfy( u => u.banned )
```

### Improved `expectAll()` Failure Messages

All collection modes now produce detailed failure summaries with pass/fail counts and per-element failure context including the element index or struct key.

```javascript
try {
    expectAll( [ 2, 4, 10, 8 ] ).toBeLT( 10 )
} catch ( any e ) {
    // e.message: "expectAll() failed: 1 of 4 element(s) did not pass the [toBeLT] expectation"
    // e.detail:  "Passed: 3 / 4\n\n[3]: The actual [10] is not less than [10]"
}
```

* * *

## Grouped Assertions: `assertAll()`

Run multiple assertion closures and report every failure at once — instead of stopping at the first. Non-assertion exceptions are rethrown immediately.

```javascript
it( "validates a user record completely", () => {
    assertAll( [
        () => expect( user.name ).notToBeEmpty(),
        () => expect( user.email ).toMatch( "@" ),
        () => expect( user.age ).toBeGTE( 18 ),
        () => expect( user.age ).toBeLT( 120 )
    ] )
} )
```

Using the `$assert` style:

```javascript
it( "xUnit style grouped assertions", () => {
    $assert.all(
        executables = [
            () => $assert.isTrue( true ),
            () => $assert.isEqual( 1, 2 ),
            () => $assert.notNull( javacast( "null", "" ) )
        ],
        heading = "Basic checks"
    )
} )
```

* * *

## New Matchers

### `toIncludeAll` / `toIncludeAny` / `toIncludeNone`

Assert that a string or array contains all, any, or none of the given needles with case-insensitive matching.

```javascript
expect( "hello world" ).toIncludeAll( [ "hello", "world" ] )
expect( "hello world" ).toIncludeAny( [ "hello", "foo" ] )
expect( "hello world" ).toIncludeNone( [ "foo", "bar" ] )
```

### `toBeTruthy` / `toBeFalsy`

Assert that a value is truthy (not false, `0`, empty string, or null) or falsy.

```javascript
expect( 42 ).toBeTruthy()
expect( "" ).toBeFalsy()
```

### `toHaveSize`

Alias for `toHaveLength()` — works on strings, arrays, structs, and queries.

```javascript
expect( "abc" ).toHaveSize( 3 )
expect( [ 1, 2 ] ).toHaveSize( 2 )
expect( { a : 1, b : 2 } ).toHaveSize( 2 )
```

### `toBeSameInstanceAs`

Assert two references point to the exact same object instance.

```javascript
var obj = { name : "test" }
expect( obj ).toBeSameInstanceAs( obj )
```

### `toThrowMatching`

Assert a function throws an exception that matches a predicate closure.

```javascript
expect( () => {
    throw( type = "FooException" )
} ).toThrowMatching( e => e.type == "FooException" )
```

* * *

## New Assertion BIFs

For xUnit-style testing, the following new `$assert` methods are available:

| Method | Description |
| --- | --- |
| `$assert.isTruthy( actual, message )` | Value is truthy |
| `$assert.isFalsy( actual, message )` | Value is falsy |
| `$assert.includesAll( target, needles, message )` | Target contains every needle |
| `$assert.includesAny( target, needles, message )` | Target contains at least one needle |
| `$assert.includesNone( target, needles, message )` | Target contains no needle |
| `$assert.all( executables, heading )` | Run all assertions, report every failure |

```javascript
$assert.isTruthy( "hello" )
$assert.isFalsy( 0 )
$assert.includesAll( "hello world", [ "hello", "world" ] )
$assert.includesAny( [ "a", "b" ], [ "b", "z" ] )
$assert.includesNone( "hello", [ "x", "y" ] )
```

* * *

## Set Expectations

TestBox now provides a comprehensive suite of set-related matchers for working with BoxLang `Set` objects. These matchers leverage the global `setOf()` function to create sets and provide powerful assertions for set operations.

### Creating Sets with `setOf()`

```javascript
var set1 = setOf( 1, 2, 3 )
var set2 = setOf( 3, 4, 5 )
var set3 = setOf( 1, 2, 3, 4, 5 )
var emptySet = setOf()
```

### `toBeASet()` / `notToBeASet()`

Assert that a value is (or is not) a Set object.

```javascript
expect( set1 ).toBeASet()
expect( [ 1, 2, 3 ] ).notToBeASet()
```

### `toEqualSet()` / `notToEqualSet()`

Assert that two sets contain the same elements, regardless of order.

```javascript
expect( setOf( 1, 2, 3 ) ).toEqualSet( setOf( 3, 2, 1 ) )
expect( setOf( 'a', 'b' ) ).toEqualSet( setOf( 'b', 'a' ) )
expect( setOf( 1, 'a', true ) ).toEqualSet( setOf( true, 'a', 1 ) )
```

### `toBeSubsetOf()` / `notToBeSubsetOf()`

Assert that all elements of the actual set are contained in the expected set.

```javascript
expect( setOf( 1, 2 ) ).toBeSubsetOf( setOf( 1, 2, 3, 4, 5 ) )
expect( setOf( 1, 2, 3, 4, 5 ) ).notToBeSubsetOf( setOf( 1, 2 ) )
```

### `toBeSupersetOf()` / `notToBeSupersetOf()`

Assert that the actual set contains all elements of the expected set.

```javascript
expect( setOf( 1, 2, 3, 4, 5 ) ).toBeSupersetOf( setOf( 1, 2, 3 ) )
expect( setOf( 1, 2, 3 ) ).notToBeSupersetOf( setOf( 1, 2, 3, 4, 5 ) )
```

### `toBeDisjointFrom()`

Assert that two sets share no common elements.

```javascript
expect( setOf( 1, 2 ) ).toBeDisjointFrom( setOf( 3, 4 ) )
```

### `toHaveUnion()` / `notToHaveUnion()`

Assert that the union of two sets equals an expected set.

```javascript
expect( setOf( 1, 2 ) ).toHaveUnion( setOf( 3, 4 ), setOf( 1, 2, 3, 4 ) )
```

### `toHaveIntersection()` / `notToHaveIntersection()`

Assert that the intersection of two sets equals an expected set.

```javascript
expect( setOf( 1, 2, 3 ) ).toHaveIntersection( setOf( 3, 4, 5 ), setOf( 3 ) )
```

### `toHaveDifference()` / `notToHaveDifference()`

Assert that the set difference (actual - expected) equals an expected result.

```javascript
expect( setOf( 1, 2, 3 ) ).toHaveDifference( setOf( 3, 4, 5 ), setOf( 1, 2 ) )
```

### `toHaveSymmetricDifference()` / `notToHaveSymmetricDifference()`

Assert that the symmetric difference (elements in either set but not both) equals an expected result.

```javascript
expect( setOf( 1, 2, 3 ) ).toHaveSymmetricDifference( setOf( 3, 4, 5 ), setOf( 1, 2, 4, 5 ) )
```

### Real-World Example: Menu Selection

```javascript
it( "validates menu selection", () => {
    var fruits = setOf( 'apple', 'banana', 'cherry' )
    var selected = setOf( 'apple', 'banana' )

    expect( selected ).toBeSubsetOf( fruits )
    expect( selected ).toHaveIntersection( fruits, setOf( 'apple', 'banana' ) )
} )
```

* * *

## Range Expectations (BoxLang)

TestBox now includes a full set of matchers for BoxLang `Range` objects, including containment, ordering, bounds, and step/clamp assertions.

{% hint style="info" %}
**BoxLang only.** Range features depend on BoxLang range support. On CFML engines these expectations are guarded and report unsupported behavior cleanly.

Ranges are created with the `..` operator (for example `1..10`, `..10`, `1..`, `..`) and stepped via `.step( n )`. There is no `rangeNew()` BIF.
{% endhint %}

### Core Range Matchers

- `toBeRange()`
- `toContainValue( value )`
- `toContainRange( range )`
- `toBeInRange( range )`
- `toBeBeforeRange( range )`
- `toBeAfterRange( range )`

### Range Shape And Direction

- `toBeBounded()`
- `toBeUnbounded()`
- `toBeHalfBounded()`
- `toBeIterable()`
- `toBeAscending()`
- `toBeDescending()`

### Step And Clamp

- `toHaveStep( step )`
- `toClampTo( value, expected )`

```javascript
var base = 1..10
var stepped = (0..100).step( 5 )
var chars = "a".."z"
var dates = createDate( 2024, 1, 1 )..createDate( 2024, 1, 31 )

expect( base ).toBeRange()
expect( base ).toContainValue( 5 )
expect( base ).toContainRange( 3..7 )
expect( 8 ).toBeInRange( base )

expect( stepped ).toHaveStep( 5 )
expect( base ).toClampTo( 15, 10 )
expect( dates ).toContainValue( "2024-01-15" )

expect( chars ).toBeAscending()
expect( chars ).toContainValue( "m" )
```

Equivalent native Range API examples used under these matchers:

```javascript
base.contains( 5 )
base.contains( 3..7 )
stepped.getStep()          // 5
base.clamp( 15 )           // 10
```

* * *

## Data Navigator Expectations

TestBox now provides a suite of matchers that leverage BoxLang's built-in `dataNavigate()` BIF to safely navigate and assert against values in nested data structures. These matchers support dot-notation, array indexes, wildcards, filters, recursive descent, and all other JSONPath-style expressions.

{% hint style="info" %}
**BoxLang only.** Data navigator features require the BoxLang runtime and are guarded at the matcher level. On CFML engines they throw `TestBox.BoxLangFeatureNotAvailable`.
{% endhint %}

### `toHavePath()` / `notToHavePath()`

Assert that a path exists (or does not exist) in a nested data structure.

```javascript
var data = {
    "app" = { "name" = "TestApp", "settings" = { "debug" = true, "port" = 8080 } },
    "users" = [ { "name" = "Alice", "age" = 30 } ]
}

expect( data ).toHavePath( "app.name" )
expect( data ).toHavePath( "app.settings.debug" )
expect( data ).toHavePath( "users[1].name" )
expect( data ).notToHavePath( "nonexistent" )
expect( data ).notToHavePath( "app.nonexistent" )
```

### `toHavePathValue()` / `notToHavePathValue()`

Assert that the value at a path matches an expected value.

```javascript
expect( data ).toHavePathValue( "app.name", "TestApp" )
expect( data ).toHavePathValue( "app.settings.port", 8080 )
expect( data ).toHavePathValue( "app.settings.debug", true )
expect( data ).toHavePathValue( "users[1].name", "Alice" )
expect( data ).notToHavePathValue( "app.name", "WrongApp" )
```

### `toHavePathType()` / `notToHavePathType()`

Assert the type of the value at a path. Supports standard types (`string`, `numeric`, `boolean`, `struct`, `array`) and common aliases (`str`, `num`, `bool`, `arr`, `obj`, `map`).

```javascript
expect( data ).toHavePathType( "app.name", "string" )
expect( data ).toHavePathType( "app.settings.port", "numeric" )
expect( data ).toHavePathType( "app.settings.debug", "boolean" )
expect( data ).toHavePathType( "app.settings", "struct" )
expect( data ).toHavePathType( "users", "array" )
expect( data ).toHavePathType( "app.settings.port", "num" )       // alias
expect( data ).notToHavePathType( "app.name", "numeric" )
```

### `toHavePathSatisfying()` / `notToHavePathSatisfying()`

Assert that the value at a path satisfies a predicate closure.

```javascript
expect( data ).toHavePathSatisfying( "app.name", name -> name == "TestApp" )
expect( data ).toHavePathSatisfying( "app.settings.port", port -> port > 1000 )
expect( data ).toHavePathSatisfying( "app.settings", settings -> settings.keyExists( "debug" ) )
expect( data ).notToHavePathSatisfying( "app.name", name -> name == "WrongName" )
```

### `path()`

Navigate to a path and return a normal `Expectation` on the value at that path. Supports chaining any matcher on the result.

```javascript
expect( data ).path( "app.name" ).toBe( "TestApp" )
expect( data ).path( "app.settings.port" ).toBeGT( 8000 )
expect( data ).path( "users" ).toHaveLength( 1 )
expect( data ).path( "nonexistent" ).toBeNull()
```

### `queryPath()`

Navigate to a path and return an `Expectation` on an array of all matching values. Fans out at wildcards, filters, and recursive descent segments.

```javascript
expect( data ).queryPath( "users[*].name" ).toHaveLength( 1 )
expect( data ).queryPath( "users[*].name" ).toInclude( "Alice" )
expect( data ).queryPath( "nonexistent" ).toBeEmpty()
```

### Real-World Example: API Response Validation

```javascript
it( "validates an API response", () => {
    var response = {
        "success" = true,
        "data" = {
            "users" = [
                { "name" = "Alice", "role" = "admin", "active" = true },
                { "name" = "Bob",   "role" = "user",  "active" = false }
            ],
            "metadata" = { "total" = 2, "page" = 1 }
        }
    }

    // Path existence
    expect( response ).toHavePath( "data.users" )
    expect( response ).notToHavePath( "data.errors" )

    // Path values
    expect( response ).toHavePathValue( "success", true )
    expect( response ).toHavePathValue( "data.metadata.total", 2 )

    // Path types
    expect( response ).toHavePathType( "data.users", "array" )
    expect( response ).toHavePathType( "data.metadata.total", "numeric" )

    // Predicate
    expect( response ).toHavePathSatisfying( "data.users[1].role", role -> role == "admin" )

    // Path extraction
    expect( response ).path( "data.users[1].name" ).toBe( "Alice" )
    expect( response ).queryPath( "data.users[*].name" ).toInclude( "Bob" )
} )
```

## Class-Level `skip` Annotation

BDD test classes now support a class-level `skip` annotation, so an entire class can be skipped without touching each `describe()` or editing the runner's filters.

```js
/**
 * @skip
 */
component extends="testbox.system.BaseSpec" {

    function run(){
        describe( "Payment gateway", function(){
            // none of this runs while @skip is present
        } );
    }

}
```

Skipped classes are reported as skipped rather than silently omitted, so the count stays honest.

You can also pass a reason:

```js
/**
 * @skip Waiting on the sandbox credentials
 */
```

## MockBox `$args()` Improvements

`$args()` previously built its argument hash in a way that depended on struct iteration order. Two structurally identical structs built in a different order could hash differently, so a mock could fail to match arguments it should have matched and would return `null` instead.

This was always latent, but Lucee 7.1 changed its underlying map implementation and made it reproducible.

`$args()` now normalizes nested structures deterministically, so structurally-equal arguments match regardless of how they were built:

```js
var mock = createMock( "PaymentService" )
    .$( "charge" )
    .$args( { amount : 100, currency : "USD" } )
    .$results( true );

// matches, even though the keys were supplied in a different order
mock.charge( { currency : "USD", amount : 100 } );
```

`$args()` also now understands BoxLang `Set` and `Range` objects when matching.

## Summary

| Feature | Type | Example |
| --- | --- | --- |
| `withContext()` | Expectation | `expect( v ).withContext( "label" ).toBe( x )` |
| `expectAny()` | Collection | `expectAny( arr ).toBeGT( 2 )` |
| `expectSome()` | Collection | `expectSome( arr, 2, 5 ).toBeGT( 2 )` |
| `expectNone()` | Collection | `expectNone( arr ).toBeEmpty()` |
| `assertAll()` | Grouped | `assertAll( closures, "heading" )` |
| `toBeTruthy()` | Matcher | `expect( v ).toBeTruthy()` |
| `toBeFalsy()` | Matcher | `expect( v ).toBeFalsy()` |
| `toBeSameInstanceAs()` | Matcher | `expect( a ).toBeSameInstanceAs( b )` |
| `toHaveSize()` | Matcher | `expect( v ).toHaveSize( 3 )` |
| `toThrowMatching()` | Matcher | `expect( fn ).toThrowMatching( p )` |
| `toIncludeAll()` | Matcher | `expect( v ).toIncludeAll( needles )` |
| `toIncludeAny()` | Matcher | `expect( v ).toIncludeAny( needles )` |
| `toIncludeNone()` | Matcher | `expect( v ).toIncludeNone( needles )` |
| `toBeASet()` / `nottoBeASet()` | Set | `expect( setOf( 1, 2 ) ).toBeASet()` |
| `toEqualSet()` / `notToEqualSet()` | Set | `expect( setOf( 1, 2 ) ).toEqualSet( setOf( 2, 1 ) )` |
| `toBeSubsetOf()` / `notToBeSubsetOf()` | Set | `expect( setOf( 1 ) ).toBeSubsetOf( setOf( 1, 2 ) )` |
| `toBeSupersetOf()` / `notToBeSupersetOf()` | Set | `expect( setOf( 1, 2 ) ).toBeSupersetOf( setOf( 1 ) )` |
| `toBeDisjointFrom()` | Set | `expect( setOf( 1 ) ).toBeDisjointFrom( setOf( 2 ) )` |
| `toHaveUnion()` / `notToHaveUnion()` | Set | `expect( setOf( 1 ) ).toHaveUnion( setOf( 2 ), setOf( 1, 2 ) )` |
| `toHaveIntersection()` / `notToHaveIntersection()` | Set | `expect( setOf( 1, 2 ) ).toHaveIntersection( setOf( 2, 3 ), setOf( 2 ) )` |
| `toHaveDifference()` / `notToHaveDifference()` | Set | `expect( setOf( 1, 2 ) ).toHaveDifference( setOf( 2 ), setOf( 1 ) )` |
| `toHaveSymmetricDifference()` / `notToHaveSymmetricDifference()` | Set | `expect( setOf( 1, 2 ) ).toHaveSymmetricDifference( setOf( 2, 3 ), setOf( 1, 3 ) )` |
| `toBeRange()` | Range | `expect( 1..10 ).toBeRange()` |
| `toContainValue()` | Range | `expect( 1..10 ).toContainValue( 5 )` |
| `toContainRange()` | Range | `expect( 1..10 ).toContainRange( 2..5 )` |
| `toBeInRange()` | Range | `expect( 5 ).toBeInRange( 1..10 )` |
| `toBeBeforeRange()` | Range | `expect( 0 ).toBeBeforeRange( 1..10 )` |
| `toBeAfterRange()` | Range | `expect( 11 ).toBeAfterRange( 1..10 )` |
| `toBeBounded()` | Range | `expect( 1..10 ).toBeBounded()` |
| `toBeUnbounded()` | Range | `expect( r ).toBeUnbounded()` |
| `toBeHalfBounded()` | Range | `expect( r ).toBeHalfBounded()` |
| `toBeIterable()` | Range | `expect( 1..10 ).toBeIterable()` |
| `toBeAscending()` | Range | `expect( 1..10 ).toBeAscending()` |
| `toBeDescending()` | Range | `expect( 10..1 ).toBeDescending()` |
| `toHaveStep()` | Range | `expect( r ).toHaveStep( 2 )` |
| `toClampTo()` | Range | `expect( 1..10 ).toClampTo( 20, 10 )` |
| `toHavePath()` / `notToHavePath()` | Navigator | `expect( data ).toHavePath( "app.name" )` |
| `toHavePathValue()` / `notToHavePathValue()` | Navigator | `expect( data ).toHavePathValue( "app.name", "TestApp" )` |
| `toHavePathType()` / `notToHavePathType()` | Navigator | `expect( data ).toHavePathType( "app.settings.port", "numeric" )` |
| `toHavePathSatisfying()` / `notToHavePathSatisfying()` | Navigator | `expect( data ).toHavePathSatisfying( "app.name", p -> p == "TestApp" )` |
| `path()` | Navigator | `expect( data ).path( "app.name" ).toBe( "TestApp" )` |
| `queryPath()` | Navigator | `expect( data ).queryPath( "users[*].name" ).toInclude( "Alice" )` |

## Bug Fixes and Improvements

### Code Coverage Is Now Opt-In

The `coverageEnabled` URL parameter in the CFML test runner now defaults to `false` instead of `true`.

Code coverage requires [FusionReactor](https://www.fusion-reactor.com/), so defaulting it on meant every plain runner hit paid for a feature most runs did not want. If you rely on coverage, enable it explicitly:

```
/tests/runner.cfm?coverageEnabled=true
```

{% hint style="warning" %}
This is a behavioral change. If you were relying on the implicit default to collect coverage, you must now pass `coverageEnabled=true` or set it in your runner options.
{% endhint %}

### Date Equality In Assertions

Equalize assertions now compare date and date/time objects by their instant rather than calling `actual.equals()` blindly. Comparing a `java.util.Date` against a CFML date string, or two date objects of different concrete types, now behaves as expected instead of failing on type identity.

### Engine Detection

`isLucee()` returned `true` under BoxLang, because BoxLang registers a `lucee` server scope key for compatibility. Any spec branching or skipping on engine took the Lucee path when running on BoxLang. `isLucee()` now requires the absence of the `boxlang` key.

### Simple Reporter HTML Escaping

Bundle and spec names are now HTML-encoded in the Simple reporter, so a test name containing markup no longer breaks the report layout.

### Full Null Support

TestBox internals assumed a non-null runtime in several places, producing spurious null-reference errors on engines configured with full null support. Null handling was audited across the runners, coverage service and mock generator, and the configuration is now covered in CI.

### BoxLang CLI Runner

Several fixes land for the BoxLang CLI runner:

- The runner no longer misreads its own script path as a positional bundle argument.
- `KeyNotFoundException [url]` no longer crashes every CLI run on BoxLang 1.17 and later. The `url` scope is not registered in CLI mode, and the runner now params it rather than referencing it directly.
- `GetPageContextResponse()` no longer errors when running BoxLang in Adobe compatibility mode.
