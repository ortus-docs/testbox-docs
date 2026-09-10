---
description: Matchers for BoxLang Range objects
---

# Range Expectations

BoxLang has a native `Range` type covering containment, ordering, bounds, stepping and clamping. TestBox 7.1 adds matchers that assert against ranges directly instead of picking them apart into endpoints first.

{% hint style="info" %}
These matchers require BoxLang. On CFML engines they are guarded and report unsupported behavior cleanly rather than erroring.
{% endhint %}

## Creating Ranges

Ranges are built with the `..` operator and stepped with `.step( n )`. There is no `rangeNew()` built-in.

```javascript
var base    = 1..10
var stepped = ( 0..100 ).step( 5 )
var chars   = "a".."z"
var dates   = createDate( 2024, 1, 1 )..createDate( 2024, 1, 31 )

var openEnd   = 1..     // half bounded
var openStart = ..10    // half bounded
var open      = ..      // unbounded
```

## Containment

### `toBeRange()`

```javascript
expect( 1..10 ).toBeRange();
```

### `toContainValue()`

Asserts a value falls inside the range.

```javascript
expect( 1..10 ).toContainValue( 5 );
expect( "a".."z" ).toContainValue( "m" );
expect( dates ).toContainValue( "2024-01-15" );
```

### `toContainRange()`

Asserts an entire range fits inside another.

```javascript
expect( 1..10 ).toContainRange( 3..7 );
```

### `toBeInRange()`

The inverse reading of `toContainValue()`, with the value as the subject. Use whichever makes the sentence read better.

```javascript
expect( 8 ).toBeInRange( 1..10 );
```

### `toBeBeforeRange()` / `toBeAfterRange()`

```javascript
expect( 0 ).toBeBeforeRange( 1..10 );
expect( 11 ).toBeAfterRange( 1..10 );
```

## Shape And Direction

### `toBeBounded()`, `toBeUnbounded()`, `toBeHalfBounded()`

```javascript
expect( 1..10 ).toBeBounded();
expect( .. ).toBeUnbounded();
expect( 1.. ).toBeHalfBounded();
```

### `toBeIterable()`

A range is iterable when it can actually be walked, which an unbounded range cannot.

```javascript
expect( 1..10 ).toBeIterable();
```

### `toBeAscending()` / `toBeDescending()`

```javascript
expect( 1..10 ).toBeAscending();
expect( 10..1 ).toBeDescending();
expect( "a".."z" ).toBeAscending();
```

## Step And Clamp

### `toHaveStep()`

```javascript
expect( ( 0..100 ).step( 5 ) ).toHaveStep( 5 );
```

### `toClampTo()`

Asserts what the range clamps a given value to. Takes the input value and the expected clamped result.

```javascript
expect( 1..10 ).toClampTo( 15, 10 );   // 15 clamps down to 10
expect( 1..10 ).toClampTo( -3, 1 );    // -3 clamps up to 1
```

## Native Equivalents

These matchers sit on top of the native Range API, so the following are equivalent to the assertions above:

```javascript
base.contains( 5 )
base.contains( 3..7 )
stepped.getStep()          // 5
base.clamp( 15 )           // 10
```

## A Real Example

```javascript
describe( "Pagination window", function(){

    it( "clamps a requested page into the available range", function(){
        var pages = 1..totalPages;

        expect( pages ).toBeRange();
        expect( pages ).toBeBounded();
        expect( pages ).toBeAscending();

        expect( pages ).toContainValue( currentPage );
        expect( pages ).toClampTo( 9999, totalPages );
    } );

} );
```
