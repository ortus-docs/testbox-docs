---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-xunit-primer/setup-and-teardown
---

# Life-Cycle Methods

TestBox not only provides you with global life-cycle methods but also with localized test methods. This is a great way to keep your tests DRY (Do not repeat yourself)!&#x20;

* `beforeTests()` - Executes once before all tests for the entire test bundle CFC
* `afterTests()` - Executes once after all tests complete in the test bundle CFC
* `setup( currentMethod )` - Executes before every single test case and receives the name of the actual testing method
* `teardown( currentMethod )` - Executes after every single test case and receives the name of the actual testing method

```javascript
component{
     function beforeTests(){}
     function afterTests(){}

     function setup( currentMethod ){}
     function teardown( currentMethod ){}
}
```

Examples

```javascript
component displayName="TestBox xUnit suite" labels="railo,cf"{

     function setup( currentMethod ){
          application.wirebox = new coldbox.system.ioc.Injector();
          structClear( request );
     }

     function teardown( currentMethod ){
          structDelete( application, "wirebox" );
          structClear( request );
     }

     function testThrows(){
          $assert.throws(function(){
               var hello = application.wirebox.getInstance( "myINvalidService" ).run();
          });
     }

     function testNotThrows(){
          $assert.notThrows(function(){
               var hello = application.wirebox.getInstance( "MyValidService" ).run();;
          });
     }

}
```
