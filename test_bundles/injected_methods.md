# Injected Methods

At runtime, TestBox will also decorate your bundle CFC with tons of methods to help you in your testing adventures. Depending on your testing style you will leverage some more than others. For the latest methods please visit the API Docs for more information.

```javascript
// quick assertion methods
assert( expression, [message] )
fail(message)

// bdd methods
beforeEach( body )
afterEach( body )
describe( title, body, labels, asyncAll, skip )
xdescribe( title, body, labels, asyncAll )
it( title, body, labels, skip )
xit( title, body, labels )
expect( actual )

// extensions
addMatchers( matchers )
addAssertions( assertions )

// runners
runRemote( testSpecs, testSuites, deubg, reporter );

// utility methods
console( var, top )
debug( var, deepCopy, label, top )
clearDebugBuffer()
getDebugBuffer()
print( message )
printLn( message )

// mocking methods
makePublic( target, method, newName )
querySim( queryData )
getmockBox( generationPath )
createEmptyMock( className, object, callLogging )
createMock( className, object, clearMethods, callLogging )
prepareMock( object, callLogging )
createStub( callLogging, extends, implements )
getProperty( target, name, scope, defaultValue )
```

