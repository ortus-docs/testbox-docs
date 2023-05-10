# BDD

A test suite begins with a call to our TestBox `describe()` function with at least two arguments: a `title` and a `body` closure within the life-cycle method called `run()`. The `title` is the name of the suite to register and the `body` function is the block of code that implements the suite. There are more arguments which you can see below:

| Argument | Required | Default | Type         | Description                                                                                                                        |
| -------- | -------- | ------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| title    | true     | ---     | string       | The title of the suite to register                                                                                                 |
| body     | true     | ---     | closure/udf  | The closure that represents the test suite                                                                                         |
| labels   | true     | ---     | string/array | The list or array of labels this suite group belongs to                                                                            |
| asyncAll | false    | false   | Boolean      | If you want to parallelize the execution of the defined specs in this suite group.                                                 |
| skip     | false    | false   | Boolean      | A flag or a closure that tells TestBox to skip this suite group from testing if true. If this is a closure it must return boolean. |

```javascript
function run( testResults, testBox ){

     describe("A suite", function(){
          it("contains spec with an awesome expectation", function(){
               expect( true ).toBeTrue();
          });
     });

}
```

In BDD, suites can be nested within each other which provides a great capability of building trees of tests. Not only does it arrange them in tree format but also TestBox will execute the life-cycle methods in order of nested suites as it traverses the tree.

```javascript
describe("A spec", function() {

     beforeEach(function() {
          coldbox = 22;
          application.wirebox = new coldbox.system.ioc.Injector();
     });

     afterEach(function() {
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

          beforeEach(function() {
               awesome = 22;
          });

          afterEach(function() {
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

## BDD Specs

Specs are defined by calling the TestBox `it()` global function, which takes in a title and a function. The title is the title of this spec or test you will write and the function is a block of code that represents the test/spec. A spec will contain most likely one or more expectations that will test the state of the SUT (software under test) or sometimes referred to as code under test.

| Argument | Required | Default | Type         | Description                                                                                                                        |
| -------- | -------- | ------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| title    | true     | ---     | string       | The title of the spec                                                                                                              |
| body     | true     | ---     | closure/udf  | The closure that represents the spec                                                                                               |
| labels   | false    | ---     | string/array | The list or array of labels this suite group belongs to                                                                            |
| skip     | false    | false   | Boolean      | A flag or a closure that tells TestBox to skip this suite group from testing if true. If this is a closure it must return boolean. |
| data     | false    | `{}`    | struct       | A struct of data you can bind the spec with so you can use within the `body` closure                                               |

An expectation is a nice assertion DSL that TestBox exposes so you can pretty much read what should happen in the testing scenario. A spec will pass if all expectations pass. A spec with one or more expectations that fail will fail the entire spec.

```javascript
function run( testResults, testBox ){

     describe("A suite", function(){
          it("contains spec with an awesome expectation", function(){
               expect( true ).toBeTrue();
          });

          it("contains a spec with more than 1 expectation", function(){
               expect( [1,2,3] ).toBeArray();
               expect( [1,2,3] ).toHaveLength( 3 );
          });
     });

}
```
