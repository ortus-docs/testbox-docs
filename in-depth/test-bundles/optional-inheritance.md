# Optional Inheritance

At runtime we provide the inheritance via mixins so you don't have to worry about it. However, if you want to declare the inheritance you can do so and this will give you the following benefits:

* Some IDEs will be able to give you introspection for methods and properties
* You will be able to use the HTML runner by executing directly the runRemote method on the CFC Bundle
* Your tests will run faster

```javascript
component extends="testbox.system.BaseSpec"{
}
```
