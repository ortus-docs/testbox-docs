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

## Nesting describe Blocks

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
