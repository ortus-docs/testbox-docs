# $throws() Method

This method is used to tell MockBox that you want to mock a method with to throw a specific exception. The exception will be thrown instead of the method returning results. This is an alternative to passing the exception in the initial `$()` call. In addition to the fluent API, the `$throws()` method also has the benefit of being able to be tied to specific `$args()` in a mocked object.

To continue with our `getKey()` example:

```javascript
configBean.getKey('DebugMode'); // Exists
configBean.getKey('OutgoingMail'); // Exists
configBean.getKey('IncmingMail'); // Does not exist (see the typo?)
```

We want to test that keys that don't exists throw a `MissingSetting` exception. Let's do that using the `$throws()` method:

```javascript
// get a mock config bean
mockConfig = getMockBox().createEmptyMock( "coldbox.system.beans.ConfigBean" );
// mock the method with args
mockConfig.$( "getKey" ).$args( "debugmode" ).$results( true );
mockConfig.$( "getKey" ).$args( "OutgoingMail" ).$results( "devmail@mail.com" );

// Here's the new $throw call
mockConfig.$( "getKey" ).$args( "IncmingMail" ).$throws( type = "MissingSetting" );

// Then you can call and get the expected results
expect( function(){
    mockConfig.getKey( "IncmingMail" );
} ).toThrow( "MissingSetting" );
```

> **Hint** Remember that the `$throws()` call must be chained to a `$()` or a `$args()` call.
