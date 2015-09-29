# $() Method

This is the method that you will call upon in order to mock a method's behavior and return results. This method has the capability of mocking a return value or even making the method throw a controlled exception. By default the mocked method results will be returned all the time the method is called. So if the mocked method is called twice, the results will always be returned.

```javascript
any $(string method, [any returns], boolean preserveReturnType='true', [boolean throwException='false'], [string throwType=''], [string throwDetail=''], [string throwMessage=''], [boolean callLogging='false'], [boolean preserveArguments='false'], [any callback])
```

Parameters:
* `method` - The method you want to mock or spy on
* `returns` - The results it must return, if not passed it returns void or you will have to do the mockResults() chain
* `preserveReturnType` - If false, the mock will make the returntype of the method equal to ANY
* `throwException` - If you want the method call to throw an exception
* `throwType` - The type of the exception to throw
* `throwDetail` - The detail of the exception to throw
* `throwMessage` - The message of the exception to throw
* `callLogging` - Will add the machinery to also log the incoming arguments to each subsequent calls to this method
* `preserveArguments` - If true, argument signatures are kept, else they are ignored. If true, BEWARE with $args() matching as default values and missing arguments need to be passed too.
* `callback` - A callback to execute that should return the desired results, this can be a UDF or closure.
* `throwErrorCode` - The error code to throw in the exception

The cool thing about this method is that it also returns the same instance of the object. Therefore, you can use it to chain calls to the object and do multiple mocking of methods all within the same line. Remember that if no returns argument is provided then the return is void

```javascript
function beforeAll(){
	mockUser = createMock("model.security.User");

    //Mock several methods all in one shot!
    mockUser.$("isFound",false).$("isDirty",false).$("isSaved",true);
}
```

#### Examples

Let's do some samples now

```javascript
//make exists return true in a mocked session object
mockSession.$(method="exists",returns=true);
expect(mockSession.exists('whatevermanKey')).toBeTrue();

//make exists return true and then false and then repeat the sequence
mockSession.$(method="exists").$results(true,false);
expect( mockSession.exists('yeaaaaa') ).toBeTrue();
expect( mockSession.exists('nada') ).toBeFalse();

//make the getVar return a mock User object
mockUser = createMock(className="model.User");
mockSession.$(method="getVar",results=mockUser);

expect( mockSession.getVar('sure') ).toBe( mockUser );

//Make the call to user.checkPermission() throw an invalid exception
mockUser.$(method="checkPermission",
		throwException=true,
		throwType="InvalidPermissionException",
		throwMessage="Invalid permission detected",
		throwDetail="The permission you sent was invalid, please try again.");

try{
	mockUser.checkPermission('invalid');
}
catch(Any e){
	if( e.type neq "InvalidPermissionException"){
	  fail('The type was invalid #e.type#');
	}
}

//mock a method with call logging
mockSession.$(method="setVar",callLogging=true);
mockSession.setVar("Hello","Luis");
mockSession.setVar("Name","luis majano");
//dump the call logs
<cfdump var="#mockSession.$callLog()#">
```




