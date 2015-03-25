# Suite Groups

As we have seen before, the `describe()` function describes a test suite of related specs in a test bundle CFC. The title of the suite is concatenated with the title of a spec to create a full spec's name which is very descriptive. If you name them well, they will read out as full sentences as defined by [BDD](http://en.wikipedia.org/wiki/Behavior-driven_development) style.

```javascript
describe("A spec", function() {
     it("is just a closure, so it can contain any code", function() {
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
     });

     it("can have more than one expectation", function() {
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
          expect( coldbox ).toBeTrue();
     });
});
```

##Setup and Teardown

If you are familiar with xUnit style frameworks, the majority of them provide a way to execute functions before and after every single test case or spec in BDD. This is a great way to keep your tests DRY (Do not repeat yourself)! TestBox provides the `beforeEach()` and `afterEach()` methods that each take in a closure as their argument that receive the name of the spec that's about to be executed or just executed. As their names indicate, they execute before a spec and after a spec in a related describe block.

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

##AroundEach() Life-Cycle Method

The `aroundEach()` life-cycle method will completely wrap your spec in another closure. This is an elegant way for you to provide a complete around AOP advice to a specification. You can use it to surround the execution in transaction blocks, ORM rollbacks, logging, and so much more. This life-cycle method will decorate ALL specs within a single suite. The method signature is below:

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

##Nesting describe Blocks

Calls to our `describe()` function can be nested with specs at any level or point of execution. This allows you to create your tests as a related tree of nested functions. Please note that before a spec is executed, TestBox walks down the tree executing each `beforeEach()` and `afterEach()` function in the declared order. This is a great way to logically group specs in any level as you see fit.

```javascript
describe("A spec", function() {

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

     describe("nested inside a second describe", function() {

          beforeEach(function( currentSpec ) {
               awesome = 22;
          });

          afterEach(function( currentSpec ) {
               awesome = 22 + 8;
          });

          it("can reference both scopes as needed ", function() {
            expect( coldbox ).toBe( awesome );
          });
     });

     it("can be declared after nested suites and have access to nested variables", function() {
          expect( awesome ).toBe( 30 );
     });

});
```

