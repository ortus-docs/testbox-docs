# Output Utilities

## Tests Output Utilities

Sometimes you will need to produce output from your tests and you can do so elegantly via some functions we have provided that are available in your test bundles:

| **Method**           | **Comment**                                  |
| -------------------- | -------------------------------------------- |
| `console()`          | Send output to the console                   |
| `debug()`            | Send output to the TestBox reporter debugger |
| `clearDebugBuffer()` | Clear the debugger                           |
| `print()`            | Send output to the ColdFusion output buffer  |
| `printLn()`          | Same as print() but adding a \<br> separator |

These are great little utilities that are needed to send output to several locations from within your tests.

{% hint style="success" %}
**Hint**: Please note that the `debug()` method does NOT do deep copies by default.
{% endhint %}

```javascript
console( myResults );

debug( myData );
debug( myData, true );

debug( var=myData, top=5 );

print( "Running This Test with #params.toString()#" );
println( "Running This Test with #params.toString()#" );
```

## Request Output Utilities

Sometimes you need to dump something that is in the CFC you are testing or maybe an asynchronous test. The aforementioned methods are only accessible from your test bundle, so getting to the TestBox output utilities is not easy. &#x20;

Since version 4.0 we have implemented the testing utilities into the `request` scope as `request.testbox.` Which will give you access to all the same output utilities:

| **Method**           | **Comment**                                  |
| -------------------- | -------------------------------------------- |
| `console()`          | Send output to the console                   |
| `debug()`            | Send output to the TestBox reporter debugger |
| `clearDebugBuffer()` | Clear the debugger                           |
| `print()`            | Send output to the ColdFusion output buffer  |
| `printLn()`          | Same as print() but adding a \<br> separator |

```javascript
request.testbox.console( "I am here" )
request.testbox.debug( "why is this not running" )
```
