# Asynchronous-Testing

You can tag a bundle component declaration with the boolean `asyncAll` annotation and TestBox will execute all specs in separate threads for you concurrently.

```javascript
component displayName="TestBox xUnit suite" skip="testEnv" asyncAll=true{

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

> **Caution** Once you delve into the asynchronous world you will have to make sure your tests are also thread safe \(var-scoped\) and provide any necessary locking.

