# xUnit

* beforeTests() - Executes once before all tests for the entire test bundle CFC
* afterTests() - Executes once after all tests complete in the test bundle CFC
* setup( currentMethod ) - Executes before every single test case and receives the name of the actual testing method
* teardown( currentMethod ) - Executes after every single test case and receives the name of the actual testing method

```javascript
component{
     function beforeTests(){}
     function afterTests(){}

     function setup( currentMethod ){}
     function teardown( currentMethod ){}
}
```

