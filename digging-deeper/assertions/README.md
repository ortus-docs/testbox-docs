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
assert( expression, [message] )
between( actual, min, max, [message] )
closeTo(expected, actual, delta, [datePart], [message])
deepKey( target, key, [message] )
fail( [message] )
includes( target, needle, [message] )
includesWithCase( target, needle, [message] )
instanceOf( actual, typeName, [message] )
isEmpty( target, [message] )
isEqual(expected, actual, [message])
isEqualWithCase(expected, actual, [message])
isFalse( actual, [message] )
isGT( actual, target, [message])
isGTE( actual, target, [message])
isLT( actual, target, [message])
isLTE( actual, target, [message])
isNotEmpty( target, [message] )
isNotEqual(expected, actual, [message])
isTrue( actual, [message] )
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
