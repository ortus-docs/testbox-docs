# Output Utilities

Sometimes you will need to produce output from your tests and you can do so elegantly via some functions we have provided that are available in your test bundles:

* console(var, top=[9999]) - Send data to the console via writeDump( output="console" )
* vdebug(var, label="", deepCopy=[false], top=999) - Send data to the TestBox debug stream, will be saved in the test results and outputted in reports
* print( message ) - Write some output to the ColdFusion output buffer
* println( message ) - Write some output to the ColdFusion output buffer and appends a

These are great little utilities that are needed to send output to several locations. Please note that the debug() method does NOT do deep copies by default.

```javascript
console( myResults );

debug( myData );
debug( myData, true );

debug( var=myData, top=5 );

print( "Running This Test with #params.toString()#" );
println( "Running This Test with #params.toString()#" );
```

Please refer to our [MockBox](http://wiki.coldbox.org/wiki/MockBox.cfm) guide to take advantage of all the mocking and stubbing you can do. However, every BDD TestBundle has the following functions available to you for mocking and stubbing purposes:

* createStub( [callLogging=true], [extends], [implements]') - Create stub objects with call logging and optional inheritance trees and implementation methods
* createEmptyMock( [className], [object], [callLogging=true]') - Create an empty mock from a class or object
* createMock( [className], [object], [clearMethods=false], [callLogging=true]') - Create a spy from an instance or class with call logging
* getMockBox( [generationPath] ') - Get a reference to MockBox
* makePublic( target, method, newName ') - Exposes private methods from objects as public methods
* querySim( queryData ') - Simulate a query
* prepareMock( object, [callLogging=true]') - Prepare an instance of an object for method spies with call logging
