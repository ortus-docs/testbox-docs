---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-xunit-primer/skipping-tests-and-suites
---

# Skipping Tests and Suites

Tests and suites can be skipped from execution by using the `skip` annotation in the component or function declaration or our `skip()` methods.   The reporters will show that these suites or tests where skipped from execution.&#x20;

### Skip Annotation

The `skip` annotation can have the following values:

* `nothing` - If you just add the annotation, we will detect it and skip the test
* `true` - Skips the test
* `false` - Does not skip the test
* `{udf_name}` - It will look for a UDF with that name, execute it and the value must evalute to boolean.

```cfscript
// Skips ALL the tests if the testEnv() returns TRUE
component displayName="TestBox xUnit suite" skip="testEnv"{

     function setup(){
          application.wirebox = new coldbox.system.ioc.Injector();
          structClear( request );
     }

     function teardown(){
          structDelete( application, "wirebox" );
          structClear( request );
     }
     
     function betaTest() skip{
          ...
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

### Skip Methods

You can also skip manually by using the `skip()` method in the `Assertion` library and also in any bundle which is inherited by the `BaseSpec` class.

#### $assert.skip( message="", detail="" )

You can use the `$assert.skip( message, detail )` method to skip any spec or suite a-la-carte instead of as an argument to the function definitions.  This lets you programmatically skip certain specs and suites and pass a nice message.

```cfscript
function testThrows(){
     $assert.skip()
     $assert.throws(function(){
          var hello = application.wirebox.getInstance( "myINvalidService" ).run()
     })
}
```

#### skip( message="", detail="" )

The `BaseSpec` has this method available to you as well.

```cfscript
it( "can use a mocked stub", function(){
    
    // If conditions met, then skip
    if( !conditionsMet() ){
        skip( "conditions for execution not met" )
    }

    c = createStub().$( "getData", 4 )
    r = calc.add( 4, c.getData() )
    expect( r ).toBe( 8 )
    expect( c.$once( "getData" ) ).toBeTrue()
    
} );
```

