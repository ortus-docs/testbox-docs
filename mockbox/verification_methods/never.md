# $never()

This method is a quick notation for the `$times(0)` call but more expressive when written in code:

```javascript
Boolean $never([methodname])
```

Parameters:

*
    methodName - The optional method name to assert the number of method calls

Examples:

```javascript
security = getMockBox().createMock("model.security");

//No calls yet
expect( security.$never() ).toBeTrue();

security.$("isValidUser",false);
security.isValidUser();

// Asserts
expect( security.$never("isValidUser") ).toBeFalse();
```

