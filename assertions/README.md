# Assertions

TestBox supports the concept of [assertions](http://en.wikipedia.org/wiki/Assertion_%28software_development%29) to allow for validations and for legacy tests. We encourage developers to use our BDD expectations as they are more readable and fun to use (Yes, fun I said!). 

The assertions are modeled in the class `testbox.system.Assertion`, so you can visit the API for the latest assertions available. Each test bundle will receive a variable called `$assert` which represents the assertions object. Here are some common assertion methods:

```javascript
fail( [message] )
assert( expression, [message] )
isTrue( actual, [message] )
isFalse( actual, [message] )
isEqual( actual, expected, [message] )
isNotEqual( actual, expected, [message] )
isEqualWithCase( actual, expected, [message] )
null( actual, [message] )
notNull( actual, [message] )
typeOf( type, actual, [message] )
notTypeOf( type, actual, [message] )
instanceOf( actual, typeName, [message] )
notInstanceOf( actual, typeName, [message] )
match( actual, regex, [message] )
matchWithCase( actual, regex, [message] )
notMatch( actual, regex, [message] )
key( target, key, [message] )
notKey( target, key, [message] )
deepKey( target, key, [message] )
notDeepKey( target, key, [message] )
lengthOf( target, length, [message] )
notLengthOf( target, length, [message] )
isEmpty( target, [message] )
isNotEmpty( target, [message] )
throws(target, [type], [regex], [message])
notThrows(target, [type], [regex], [message])
closeTo( actual, expected, delta, [datePart], [message] )
between( actual, min, max, [message] )
includes( target, needle, [message] )
notIncludes( target, needle, [message] )
includesWithCase( target, needle, [message] )
notIncludesWithCase( target, needle, [message] )
isGT( target, [message])
isGTE( target, [message])
isLT( target, [message])
isLTE( target, [message])
```

