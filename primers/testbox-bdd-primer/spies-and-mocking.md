# Spies & Mocking

Please refer to our [MockBox](../../mocking/mockbox/) section to take advantage of all the mocking and stubbing you can do. However, every BDD TestBundle has the following functions available to you for mocking and stubbing purposes:

* `makePublic( target, method, newName )` - Exposes private methods from objects as public methods
* `querySim( queryData )` - Simulate a query
* `getMockBox( [generationPath] )` - Get a reference to MockBox
* `createEmptyMock( [className], [object], [callLogging=true])` - Create an empty mock from a class or object
* `createMock( [className], [object], [clearMethods=false], [callLogging=true])` - Create a spy from an instance or class with call logging
* `prepareMock( object, [callLogging=true])` - Prepare an instance of an object for method spies with call logging
* `createStub( [callLogging=true], [extends], [implements])` - Create stub objects with call logging and optional inheritance trees and implementation methods
* `getProperty( target, name, [scope=variables], [defaultValue] )` - Get a property from an object in any scope

