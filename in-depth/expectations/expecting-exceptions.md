# Expecting Exceptions

Our default syntax for expecting exceptions is to use our closure approach concatenated with our `toThrow()` method in our expectations or our `throws()` method in our assertions object.

> **Info** Please always remember to pass in a closure to these methods and not the actual test call: `function(){ myObj.method();}`

**Example**

```javascript
expect( function(){ myObj.method(); } ).toThrow( [type], [regex], [message] );
$assert.throws( function(){ myObj.method; }, [type], [regex], [message] )
```

This will execute the closure in a nested `try/catch` block and make sure that it either threw an exception, threw with a type, threw with a type and a regex match of the exception message. If you are in an environment that does not support closures then you will need to create a spec testing function that either uses the `expectedException` annotation or function call:

```javascript
function testMyObj(){
     expectedException( [type], [regex], [message] );
}

function testMyObj() expectedException="[type]:[regex]"{
     // this function should produce an exception
}
```

> **Caution** Please note that the usage of the `expectedException()` method can ONLY be used while in synchronous mode. If you are running your tests in asynchronous mode, this will not work. We would recommend the closure or annotation approach instead.

