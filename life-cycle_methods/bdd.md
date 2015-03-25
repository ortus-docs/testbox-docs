# BDD

* `beforeAll()` - Executes once before all specs for the entire test bundle CFC
* `afterAll()` - Executes once after all specs complete in the test bundle CFC
* `run( testResults, TestBox )` - Executes once so it can capture all your describe and it blocks
* `beforeEach( currentSpec )` - Executes before every single spec in a single describe block and receives the currently executing spec.
* `afterEach( currentSpec )` - Executes after every single spec in a single describe block and receives the currently executing spec.
* `aroundEach( spec, suite )` - Executes around the executing spec so you can provide code surrouding the spec.

```javascript


component{
     function beforeAll(){}
     function afterAll(){}

     function run( testResults, testBox ){
          describe("A Spec", function(){

               beforeEach( function( currentSpec ){
                    // before each spec in this suite
               });

               afterEach( function( currentSpec ){
                    // after each spec in this suite
               });

               aroundEach( function( spec, suite ){
                    // execute the spec
                    arguments.spec.body();
               });

               describe("A nested suite", function(){

                    beforeEach( function(){
                         // before each spec in this suite + my parent's beforeEach()
                    });

                    afterEach( function(){
                         // after each spec in this suite + my parent's afterEach()
                    });

                });

          });

          describe("A second spec", function(){

               beforeEach( function( currentSpec ){
                    // before each spec in this suite, separate from the two other ones
               });

               afterEach( function( currentSpec ){
                    // after each spec in this suite, separate from the two other ones
               });

          });
     }
}
```

The great flexibility of the BDD approach is that it allows you to nest `describe` blocks or create multiple `describe` blocks. Each `describe` block can have its own life-cycle methods as well. Not only that, if they are nested, TestBox will walk the tree and call each `beforeEach()` and `afterEach()` in the order you declare them.


