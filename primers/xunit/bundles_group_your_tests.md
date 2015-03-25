# Bundles: Group Your Tests

TestBox relies on the fact of creating testing bundles which are basically CFCs. A bundle CFC will hold all the tests the TestBox runner will execute and produce reports on. Thus, sometimes this test bundle is referred to as a test suite in xUnit terms.


```javascript
component displayName="My test suite" extends="testbox.system.BaseSpec"{
     
     // executes before all tests
     function beforeTests(){}

     // executes after all tests
     function afterTests(){}

}
```

This bundle can contain 2 life-cycle methods, the `beforeTests()` and `afterTests()` methods will execute once before ALL your tests run and then after ALL your tests run. This is a great way to do any kind of global setup or teardown in your tests. It also contains a `displayName` argument in the component declaration which gives you a way to name your testing suite. We will see later the other annotations you can add to the component declaration.

