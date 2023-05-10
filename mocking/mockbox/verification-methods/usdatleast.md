# $atLeast()

This method can help you verify that at least a minimum number of calls have been made to all mocked methods or a specific mocked method.

```javascript
Boolean $atLeast(minNumberOfInvocations,[methodname])
```

Parameters:

* minNumberOfInvocations - The min number of calls to assert
* methodName - The optional method name to assert the number of method calls

```javascript
// let's say we have a service that verifies user credentials
// and if not valid, then tries to check if the user can be inflated from a cookie
// and then verified again
function verifyUser(){

    if( isValidUser() ){
        log.info("user is valid, doing valid operations");
    }

    // check if user cookie exists
    if( isUserCookieValid() ){
        // inflate credentials
        inflateUserFromCookie();
        // Validate them again
        if( NOT isValidUser() ){
            log.error("user from cookie invalid, aborting");
        }
    }
}

// Now the test
it( "can verify a user", function(){
    security = createMock("model.security").$("isValidUser",false);
    security.storeUserCookie("invalid");
    security.verifyUser();

    // Asserts that isValidUser() has been called at least 5 times
    expect( security.$atLeast(5,"isValidUser") ).toBeFalse();
    // Asserts that isValidUser() has been called at least 2 times
    expect( security.$atLeast(2,"isValidUser") ).toBeFalse();
});
```
