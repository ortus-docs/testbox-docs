# Before and After Specs

If you are familiar with xUnit style frameworks, the majority of them provide a way to execute functions before and after every single test case or spec in BDD. This is a great way to keep your tests DRY (Do not repeat yourself)! TestBox provides the `beforeEach()` and `afterEach()` methods that each take in a closure as their argument that receive the name of the spec that's about to be executed or just executed. As their names indicate, they execute before a spec and after a spec in a related `describe` block.

```javascript
describe("A spec (with setup and tear-down)", function() {

     beforeEach(function( currentSpec ) {
          coldbox = 22;
          application.wirebox = new coldbox.system.ioc.Injector();
     });

     afterEach(function( currentSpec ) {
          coldbox = 0;
          structDelete( application, "wirebox" );
     });

     it("is just a function, so it can contain any code", function() {
          expect( coldbox ).toBe( 22 );
     });

     it("can have more than one expectation and talk to scopes", function() {
          expect( coldbox ).toBe( 22 );
          expect( application.wirebox.getInstance( 'MyService' ) ).toBeComponent();
     });
});
```

#Around Specs

The `aroundEach()` life-cycle method will completely wrap your spec in another closure. This is an elegant way for you to provide a complete around AOP advice to a specification. You can use it to surround the execution in transaction blocks, ORM rollbacks, logging, and so much more. This life-cycle method will decorate ALL specs within a single suite and any children suites. The method signature is below:

```javascript
aroundEach( function( spec, suite ){
     // execute the spec
     arguments.spec.body();
} );
```

The method receives a structure of data representing the spec. This contains the following elements:

* **body** : The actual closure for the spec that you will use to execute within it.
* **labels** : The labels used in the spec
* **name** : The name of the spec
* **order** : The order of execution of the spec
* **skip** : The skip flag or closure that determines if the spec runs

The method also receives a structure of metadata about the suite this spec is contained in. It has information about life-cycle closures, async information, names, parents, etc. Here is a very practical example of creating and around each closure to provide rollbacks for specs:

```javascript
aroundEach( function( spec, suite ){

     transaction action="begin"{
          try{/
               // execute the spec
               arguments.spec.body();
          } catch( Any e ){
               rethrow;
          } finally{
               transaction action="rollback";
          }
     }
} );
```

This simple around each life-cycle closure will rollback ALL my spec's executions even if they throw exceptions.


## Annotations
TestBox since v2.3.0 also allows you to declare life-cycle methods via annotations.  Please see our [Annotations](../../life-cycle_methods/annotations.md) section for more information.