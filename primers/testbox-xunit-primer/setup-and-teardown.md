# Setup and Teardown

TestBox not only provides you with global life-cycle methods but also with localized test methods. This is a great way to keep your tests DRY (Do not repeat yourself)! TestBox provides the setup( currentMethod ) and teardown( currentMethod ) methods that each receives the name of the test method in question. which as their names indicate, they execute before a test and after a test in a test bundle.

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
