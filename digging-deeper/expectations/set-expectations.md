---
description: Matchers for BoxLang Set objects
---

# Set Expectations

BoxLang ships a native `Set` type. Asserting against one with array matchers means sorting first and hoping the ordering is stable, which obscures what the test actually means. TestBox 7.1 adds a family of matchers that speak set semantics directly.

{% hint style="info" %}
These matchers require BoxLang. They are not available on Lucee or Adobe ColdFusion.
{% endhint %}

## Creating Sets

Use the `setOf()` built-in to build a set inline:

```javascript
var roles = setOf( "admin", "editor", "viewer" );
```

## Membership And Equality

### `toBeASet()`

Asserts the actual value is a `Set`.

```javascript
expect( setOf( 1, 2 ) ).toBeASet();
expect( [ 1, 2 ] ).notToBeASet();
```

### `toEqualSet()`

Asserts two sets contain the same members. Order is irrelevant, which is the whole point of a set.

```javascript
expect( setOf( 1, 2 ) ).toEqualSet( setOf( 2, 1 ) );
```

## Subsets And Supersets

### `toBeSubsetOf()` / `toBeSupersetOf()`

```javascript
expect( setOf( "admin" ) ).toBeSubsetOf( setOf( "admin", "editor" ) );
expect( setOf( "admin", "editor" ) ).toBeSupersetOf( setOf( "admin" ) );
```

Use these for permission checks, where the assertion is "the granted roles include at least these" rather than an exact match.

### `toBeDisjointFrom()`

Asserts the two sets share no members at all.

```javascript
expect( setOf( "read" ) ).toBeDisjointFrom( setOf( "write", "delete" ) );
```

## Set Algebra

Each of these takes the other operand and the expected result.

### `toHaveUnion()`

```javascript
expect( setOf( 1 ) ).toHaveUnion( setOf( 2 ), setOf( 1, 2 ) );
```

### `toHaveIntersection()`

```javascript
expect( setOf( 1, 2 ) ).toHaveIntersection( setOf( 2, 3 ), setOf( 2 ) );
```

### `toHaveDifference()`

Members in the actual set that are not in the other set.

```javascript
expect( setOf( 1, 2 ) ).toHaveDifference( setOf( 2 ), setOf( 1 ) );
```

### `toHaveSymmetricDifference()`

Members in either set but not both.

```javascript
expect( setOf( 1, 2 ) ).toHaveSymmetricDifference( setOf( 2, 3 ), setOf( 1, 3 ) );
```

## Negated Forms

Every matcher here has a negated counterpart: `notToBeASet()`, `notToEqualSet()`, `notToBeSubsetOf()`, `notToBeSupersetOf()`, `notToHaveUnion()`, `notToHaveIntersection()`, `notToHaveDifference()` and `notToHaveSymmetricDifference()`.

## A Real Example

```javascript
describe( "Menu permissions", function(){

    it( "shows only the menu items the user may reach", function(){
        var visible = menuService.visibleFor( user );
        var granted = setOf( "dashboard", "reports" );

        expect( visible ).toBeASet();
        expect( visible ).toEqualSet( granted );
        expect( visible ).toBeDisjointFrom( setOf( "admin", "billing" ) );
    } );

} );
```
