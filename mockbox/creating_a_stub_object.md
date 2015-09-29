# Creating a Stub Object

In order to create a stub object you will use the : `createStub()` method.

`public any createStub([boolean callLogging='true'], [extends], [implements])`

Parameters:
* `callLogging` - Add method call logging for all mocked methods
* `extends` - Make the stub extend from certain CFC
* `implement` - Make the stub adhere to an interface(s)

This method will create an empty stub object that you can use and mock with methods and properties. It can then be used in any code to satisfy dependencies meanwhile you build them. This is great to start working on projects where you are relying on other teams to build functionality but you have agreed on specific data or code interfaces. It is also super fancy as it can allow you to tell the stub to inherit from another CFC and look like it, or even pass in one or more interfaces that it must implement. If they are interfaces, then MockBox will generate all the necessary methods to satisfy those interfaces.


## CreateStub() Inheritance

The `createStub()` method has an argument called `extends` that accepts a class path. This will create and generate a stub that physically extends that class path directly. This is an amazing way to create stubs that you can override with inherited functionality or just make it look like it is EXACTLY the type of object you want.

```javascript
myService = mockbox.createStub(extends="model.security.MyService");
```

## CreateStub() Interfaces

The `createStub()` method has an argument called `implements` that accepts a list of interface class paths you want the stub to implement. MockBox will then generate the stub and it will make sure that it implements all the methods the interfaces have defined as per their contract. This is such a fantastic and easy way to create a stub that looks and feels and actually has the methods an interface needs.

```javascript
myFakeProvider = mockbox.createStub(implements="coldbox.system.cache.ICacheProvider");
myFakeProvider.getName();
```


