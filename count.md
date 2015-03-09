# $count()

Get the number of times a method has been called or the entire number of calls made to ANY mocked method on this mock object. If the method has never been called, you will receive a 0. If the method does not exist or has not been mocked, then it will return a -1.

```javascript
numeric $count([string methodName])
```

Parameters:
* methodName - Name of the method to get the counter for (Optional)

```javascript
mockUser = getMockBox().createMock("model.User");
mockUser.$("isAuthorized").$results(false,true);

debug(mockUser.$count("isAuthorized"));
//DUMPS 0

mockUser.isAuthorized();
debug(mockUser.$count("isAuthorized"));
//DUMPS 1

mockUser.isAuthorized();
debug(mockUser.$count("isAuthorized"));
//DUMPS 2

// dumps 2 also
debug( mockUser.$count() );
```


