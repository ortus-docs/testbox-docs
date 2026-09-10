---
icon: octagon-check
metaLinks:
  alternates:
    - https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/digging-deeper/assertions
---

# Assertions

TestBox supports the concept of [assertions](http://en.wikipedia.org/wiki/Assertion_\(software_development\)) to allow for validations and for legacy tests. We encourage developers to use our BDD expectations as they are more readable and fun to use (Yes, fun I said!).

The assertions are modeled in the class `testbox.system.Assertion`, so you can visit the [API](http://apidocs.ortussolutions.com/testbox/current/?testbox/system/Assertion.html) for the latest assertions available. Each test bundle will receive a variable called `$assert` which represents the assertions object.&#x20;

### BoxLang Assertions

If you are running and testing with BoxLang, you will have the extra benefit of the assertions dynamic methods.  This allows you to just called the method in the `Assertion` object prefixed by `assert`.

```cfscript
// Normal method
$assert.isTrue()
$assert.between()
$assert.closeTo()

// With BoxLang Dynamic Methods
assertIsTrue()
assertBetween()
assertCloseTo()
```

### Common Assertions

Here are some common assertion methods:

```javascript
all( closures, [heading] )
assert( expression, [message] )
between( actual, min, max, [message] )
closeTo(expected, actual, delta, [datePart], [message])
deepKey( target, key, [message] )
fail( [message] )
includes( target, needle, [message] )
includesAll( target, needles, [message] )
includesAny( target, needles, [message] )
includesNone( target, needles, [message] )
includesWithCase( target, needle, [message] )
instanceOf( actual, typeName, [message] )
isEmpty( target, [message] )
isEqual(expected, actual, [message])
isEqualWithCase(expected, actual, [message])
isFalse( actual, [message] )
isFalsy( actual, [message] )
isGT( actual, target, [message])
isGTE( actual, target, [message])
isLT( actual, target, [message])
isLTE( actual, target, [message])
isNotEmpty( target, [message] )
isNotEqual(expected, actual, [message])
isTrue( actual, [message] )
isTruthy( actual, [message] )
key( target, key, [message] )
lengthOf( target, length, [message] )
match( actual, regex, [message] )
matchWithCase( actual, regex, [message] )
notDeepKey( target, key, [message] )
notIncludes( target, needle, [message] )
notIncludesWithCase( target, needle, [message] )
notInstanceOf( actual, typeName, [message] )
notKey( target, key, [message] )
notLengthOf( target, length, [message] )
notMatch( actual, regex, [message] )
notNull( actual, [message] )
notThrows(target, [type], [regex], [message])
notTypeOf( type, actual, [message] )
null( actual, [message] )
skip( message, detail )
throws(target, [type], [regex], [message])
typeOf( type, actual, [message] )
```

### Truthiness

`isTrue()` and `isFalse()` require an actual boolean. `isTruthy()` and `isFalsy()` are looser, and are useful when the value under test is "something or nothing" rather than a strict boolean.

```javascript
$assert.isTruthy( "hello" );
$assert.isTruthy( [ 1, 2 ] );

$assert.isFalsy( "" );
$assert.isFalsy( 0 );
$assert.isFalsy( [] );
```

### Multiple Inclusions

`includes()` checks for one needle. These three check for several at once:

```javascript
$assert.includesAll( roles, [ "admin", "editor" ] );
$assert.includesAny( roles, [ "admin", "superuser" ] );
$assert.includesNone( serializedUser, [ "password", "salt", "apiToken" ] );
```

### Grouped Assertions

By default a failing assertion aborts the test, so you only ever see the first failure and fix them one run at a time. `$assert.all()` runs a set of assertion closures and reports **every** failure at once.

```javascript
$assert.all( [
    () => $assert.isEqual( "Luis", user.getName() ),
    () => $assert.isEqual( "luis@ortussolutions.com", user.getEmail() ),
    () => $assert.isTrue( user.isActive() )
], "user profile" );
```

If the name and the active flag are both wrong, both are reported:

```
user profile: 1 of 3 assertions passed
  [1] expected [Luis] but received [Alice]
  [3] expected [true] but received [false]
```

The optional second argument is a heading prepended to the failure summary.

`assertAll()` is available as a spec-level shortcut for the same thing:

```javascript
assertAll( [
    () => $assert.isEqual( 200, response.status ),
    () => $assert.key( response, "data" )
], "response envelope" );
```

{% hint style="info" %}
Grouped assertions are the assertion-style counterpart to [collection expectations](../expectations/#collection-expectations). Reach for them when several independent facts about one object should all be reported together.
{% endhint %}
