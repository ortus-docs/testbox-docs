# $times() or $verifyCallCount()

This method is used to assert how many times a mocked method has been called or ANY mocked method has been called.

```javascript
Boolean $times(numeric count, [methodname])
```

Parameters:
* count - The number of times any method or a specific mocked method has been called
* methodName - The optional method name to assert the number of method calls

Examples

```javascript
security = getMockBox().createMock("model.security");

//No calls yet
expect( security.$times(0) ).toBeTrue();

security.$("isValidUser",false);
security.isValidUser();

// Asserts
expect( security.$times(1) ).toBeTrue();
expect( security.$times(1,"isValidUser") ).toBeTrue();

security.$("authenticate",true);
security.authenticate("username","password");

expect( security.$times(2) ).toBeTrue();
expect( security.$times(1,"authenticate") ).toBeTrue();
```
