# Skipping Tests and Suites

Tests and suites can be skipped from execution by using the skip annotation in the component or function declaration. The reporters will show that these suites or tests where skipped from execution. The value of the skip annotation can be a simple true or false or it can be the name of a UDF that exists in the same bundle CFC. This UDF must return a boolean value and it is evaluated at runtime.

```javascript
component displayName="TestBox xUnit suite" skip="testEnv"{

     function setup(){
          application.wirebox = new coldbox.system.ioc.Injector();
          structClear( request );
     }

     function teardown(){
          structDelete( application, "wirebox" );
          structClear( request );
     }

     function testThrows() skip="true"{
          $assert.throws(function(){
               var hello = application.wirebox.getInstance( "myINvalidService" ).run();
          });
     }

     function testNotThrows(){
          $assert.notThrows(function(){
               var hello = application.wirebox.getInstance( "MyValidService" ).run();;
          });
     }

     private boolean function testEnv(){
          return ( structKeyExists( request, "env") && request.env == "stg" ? true : false );
     }

}
```

## $assert.skip()

You can use the `$assert.skip( message, detail )` method to skip any spec or suite a-la-carte instead of as an argument to the function definitions.  This lets you programmatically skip certain specs and suites and pass a nice message.

```cfscript
function testThrows(){
     $assert.skip();
     $assert.throws(function(){
          var hello = application.wirebox.getInstance( "myINvalidService" ).run();
     });
}
```
