# Output Utilities

Sometimes you will need to produce output from your tests and you can do so elegantly via some functions we have provided that are available in your test bundles:

* `console(var, top=[9999])` - Send data to the console via `writeDump( output="console" )`
* `debug(var, label="", deepCopy=[false], top=999)` - Send data to the TestBox debug stream, will be saved in the test results and outputted in reports
* `print( message )` - Write some output to the ColdFusion output buffer
* `println( message )` - Write some output to the ColdFusion output buffer and appends a newline character

These are great little utilities that are needed to send output to several locations.

> **Hint** Please note that the `debug()` method does **NOT** do deep copies by default.

```javascript
console( myResults );

debug( myData );
debug( myData, true );

debug( var=myData, top=5 );

print( "Running This Test with #params.toString()#" );
println( "Running This Test with #params.toString()#" );
```

