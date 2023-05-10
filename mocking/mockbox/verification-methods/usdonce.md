# $once()

This method can help you verify that only ONE mocked method call has been made on the entire mock or a specific mocked method. Useful alias!

```javascript
Boolean $once([methodname])
```

Parameters:

* methodName - The optional method name to assert the number of method calls

Examples:

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
    security = getMockBox().createMock("model.security").$("isValidUser",false);
    security.storeUserCookie("valid");
    security.verifyUser();

    expect( security.$once("isValidUser") ).toBeTrue();
});
```
