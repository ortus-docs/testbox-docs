# $property() Method
This method is used in order to mock an internal property on the target object. Let's say that the object has a private property of userDAO that lives in the variables scope and the lifecycle for the object is controlled by its parent, in this case the user service. This means that this dependency is created by the user service and not injected by an external force or dependency injection framework. How do we mock this? Very easily by using the $property() method on the target object.

```javascript
any $property(string propertyName, [string propertyScope='variables'], any mock)
```

Parameters:

* propertyName - The name of the property to mock
* propertyScope - The scope where the property lives in. The default is variables scope.
* mock - The object or data to inject and mock


```javascript
//decorate our user service with mocking capabilities, just to show a different approach
userService = getMockBox().prepareMock( createObject("component","model.UserService") );

//create a mock dao and mock the getUsers() method
mockDAO=getMockBox().createEmptyMock("model.UserDAO").$("getUsers",QueryNew(""));

//Inject it as a property of the user service, since no external injections are found. variables scope is the default.
userService.$property(propertyName="userDAO",mock=mockDAO);

//Test a user service method that uses the DAO
results = userService.getUsers();
assertTrue( isQuery(results) );
```

Not only can you mock properties that are objects, but also mock properties that are simple/complex types. Let's say you have a property in your target object that controls debugging and by default the property is false, but you want to test the debugging capabilities of your class. So we have to mock it to true now, but the property exists in variables.instance.debugMode? No problem mate (Like my friend Mark Mandel says)!

```javascript
//decorate the cache object with mocking capabilties
cache = getMockBox().createMock(object=createObject("component","MyCache"));

//mock the debug property
cache.$property(propertyName="debugMode",propertyScope="instance",mock=true);
```



