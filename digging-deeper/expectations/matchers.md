---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/digging-deeper/expectations/matchers
---

# Matchers

The `toBe()` matcher represents an equality matcher much how an `$assert.isEqual()` behaves. Below are several of the most common matchers available to you. However, the best way to see which ones are available is to checkout the [API Docs](http://apidocs.ortussolutions.com/testbox/current).

```javascript
toBeTrue( [message] ) :  value to true
toBeFalse( [message] ) : value to be false
toBe( expected, [message] ) : Assert something is equal to each other, no case is required
toBeWithCase( expected, [message] ) : Expects with case
toBeNull( [message] ) : Expects the value to be null
toBeInstanceOf( class, [message] ) : To be the class instance passed
toMatch( regex, [message] ) : Matches a string with no case-sensitivity
toMatchWithCase( regex, [message] ) : Matches with case-sensitivity
toBeTypeOf( type, [message] ) : Assert the type of the incoming actual data, it uses the internal ColdFusion isValid() function behind the scenes, type can be array, binary, boolean, component, date, time, float, numeric, integer, query, string, struct, url, uuid plus all the ones from isValid()
toBe{type}( [message] ) : Same as above but more readable method name. Example: .toBeStruct(), .toBeArray()
toBeEmpty( [message] ) : Tests if an array or struct or string or query is empty
toHaveKey( key, [message] ) : Tests the existence of one key in a structure or hash map
toHaveDeepKey( key, [message] ) : Assert that a given key exists in the passed in struct by searching the entire nested structure
toHaveLength( length, [message] ) : Assert the size of a given string, array, structure or query
toThrow( [type], [regex], [message] );
toBeCloseTo( expected, delta, [datepart], [message] ) : Can be used to approximate numbers or dates according to the expected and delta arguments.  For date ranges use the datepart values.
toBeBetween( min, max, [message] ) : Assert that the passed in actual number or date is between the passed in min and max values
toInclude( needle, [message] ) : Assert that the given "needle" argument exists in the incoming string or array with no case-sensitivity, needle in a haystack anyone?
toIncludeWithCase( needle, [message] ) : Assert that the given "needle" argument exists in the incoming string or array with case-sensitivity, needle in a haystack anyone?
toBeGT( target, [message] ) : Assert that the actual value is greater than the target value
toBeGTE( target, [message] ) : Assert that the actual value is greater than or equal the target value
toBeLT( target, [message] ) : Assert that the actual value is less than the target value
toBeLTE( target, [message] ) : Assert that the actual value is less than or equal the target value
toBeTruthy( [message] ) : Assert the value is truthy: not false, not zero, not an empty string, not null
toBeFalsy( [message] ) : Assert the value is falsy: false, zero, an empty string or null
toBeSameInstanceAs( expected, [message] ) : Assert both references point at the very same object instance, not merely equal values
toHaveSize( expected, [message] ) : Assert the size of an array, struct, string or query. Alias of toHaveLength() reading more naturally for collections
toThrowMatching( predicate, [message] ) : Assert an exception is thrown AND that it satisfies the passed closure/lambda predicate
toIncludeAll( needles, [message] ) : Assert the target contains every one of the passed values
toIncludeAny( needles, [message] ) : Assert the target contains at least one of the passed values
toIncludeNone( needles, [message] ) : Assert the target contains none of the passed values
```

{% hint style="info" %}
Every matcher above has a negated counterpart via the [not operator](not-operator.md), for example `expect( x ).notToHaveSize( 3 )`.
{% endhint %}

## Truthiness: `toBeTruthy()` and `toBeFalsy()`

`toBeTrue()` and `toBeFalse()` require an actual boolean. `toBeTruthy()` and `toBeFalsy()` are looser, and are useful when a function returns "something or nothing" rather than a strict boolean.

```javascript
expect( "hello" ).toBeTruthy();
expect( [ 1, 2 ] ).toBeTruthy();
expect( 1 ).toBeTruthy();

expect( "" ).toBeFalsy();
expect( 0 ).toBeFalsy();
expect( [] ).toBeFalsy();
```

## Identity: `toBeSameInstanceAs()`

`toBe()` compares values. `toBeSameInstanceAs()` compares identity, which is what you want when asserting that a singleton really is a singleton, or that a factory handed back the cached object rather than a fresh one.

```javascript
var a = getInstance( "UserService" );
var b = getInstance( "UserService" );

expect( a ).toBeSameInstanceAs( b );      // same object in memory
expect( a ).notToBeSameInstanceAs( {} );
```

## Size: `toHaveSize()`

Works on arrays, structs, strings and queries.

```javascript
expect( [ 1, 2, 3 ] ).toHaveSize( 3 );
expect( { a : 1, b : 2 } ).toHaveSize( 2 );
expect( "TestBox" ).toHaveSize( 7 );
```

## Exceptions: `toThrowMatching()`

`toThrow()` matches on exception type and a message regex. `toThrowMatching()` hands you the exception so you can assert anything about it.

```javascript
expect( function(){
    paymentService.charge( amount = -5 );
} ).toThrowMatching( function( e ){
    return e.type == "InvalidAmount" && e.detail contains "negative";
} );
```

This is the escape hatch for exceptions whose interesting detail is not in the type or the message: a custom `extendedInfo` payload, an error code, a nested cause.

## Collections: `toIncludeAll()`, `toIncludeAny()`, `toIncludeNone()`

`toInclude()` checks for a single needle. These three check for several at once against arrays, lists and strings.

```javascript
expect( [ "admin", "editor", "viewer" ] ).toIncludeAll( [ "admin", "editor" ] );
expect( [ "admin", "viewer" ] ).toIncludeAny( [ "admin", "superuser" ] );
expect( [ "viewer" ] ).toIncludeNone( [ "admin", "superuser" ] );
```

Use `toIncludeNone()` to assert the absence of things that must never leak, which reads better than chaining several negated `toInclude()` calls:

```javascript
expect( serializedUser ).toIncludeNone( [ "password", "salt", "apiToken" ] );
```

## Specialized Matcher Families

TestBox 7.1 adds three dedicated matcher families with their own pages:

- [Set Expectations](set-expectations.md) for BoxLang `Set` objects
- [Range Expectations](range-expectations.md) for BoxLang `Range` objects
- [Data Navigator Expectations](data-navigator.md) for asserting against deeply nested structures by path
