# Test and Suite Labels
Tests and suites can be tagged with TestBox labels. Labels allows you to further categorize different tests or suites so that when a runner executes with labels attached, only those tests and suites will be executed, the rest will be skipped. Labels can be applied globally to the component declaration of the test bundle suite or granularly at the test method declaration.

```javascript
component displayName="TestBox xUnit suite" labels="railo,stg,dev"{

     function setup(){
          application.wirebox = new coldbox.system.ioc.Injector();
          structClear( request );
     }

     function teardown(){
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

     function testFailsShortcut() labels="dev"{
          fail( "This Test should fail when executed with labels" );
     }

}
```

