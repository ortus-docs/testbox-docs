# $args() Method

This method is used to tell MockBox that you want to mock a method with a SPECIFIC number of argument calls. Then you will have to set the return results for it, but this is absolutely necessary if you need to test an object that makes several method calls to the same method with different arguments, and you need to mock different results coming back. Example, let's say you are using a ColdBox configuration bean that holds configuration data. You make several calls to the `getKey()` method with different arguments:

```javascript
configBean.getKey('DebugMode');
configBean.getKey('OutgoingMail');
```

How in the world can I mock ths? Well, using the mock arguments method.

```javascript
//get a mock config bean
mockConfig = getMockBox().createEmptyMock("coldbox.system.beans.ConfigBean");
//mock the method with args
mockConfig.$("getKey").$args("debugmode").$results(true);
mockConfig.$("getKey").$args("OutgoingMail").$results('devmail@mail.com');

//Then you can call and get the expected results
```

So remember that if you use the `$args()` call, you need to tell it what kind of results you are expecting by calling the `$results()` method after it or you might end up with an exception.

