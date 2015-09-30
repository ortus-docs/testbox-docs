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

##Given-When-Then Blocks

[Given-When-Then](http://martinfowler.com/bliki/GivenWhenThen.html) is a style of writing tests where you describe the state of the code you want to test (`Given`), the behavior you want to test (`When`) and the expected outcome (`Then`). (See [Specification By Example](http://martinfowler.com/bliki/SpecificationByExample.html))

Testbox supports the use of function names `given()` and `when()` in-place of `describe()` function calls. The `then()` function call is an alternative for `it()` function calls. The advantage of this style of [behavioural specifications](https://en.wikipedia.org/wiki/Behavior-driven_development#Behavioural_specifications) is that you can gather your requirements and write your tests in a common language that can be understood by developers and stake-holders alike. This common language format is often referred to as the [Gherkin](https://github.com/cucumber/cucumber/wiki/Gherkin) language; using it we can gather and document the requirements as: 

```
Feature: Box Size
    In order to know what size box I need
    As a distribution manager
    I want to know the volume of the box

    Scenario: Get box volume
        Given I have entered a width of 20
        And a height of 30
        And a depth of 40
        When I run the calculation
        Then the result should be 24000
```

TestBox provides you with `feature()`, `scenario()` and `story()` wrappers for `describe()` blocks. As such we can write our requirements in test form like so:

```javascript
feature( "Box Size", function(){

    describe( "In order to know what size box I need
              As a distribution manager
              I want to know the volume of the box", function(){

        scenario( "Get box volume", function(){
            given( "I have entered a width of 20
                And a height of 30
                And a depth of 40", function(){
                when( "I run the calculation", function(){
		              then( "the result should be 24000", function(){
		                  // call the method with the arguments and test the outcome
		                  expect( myObject.myFunction(20,30,40) ).toBe( 24000 );
		              });
	             });
            });
        });
    });
});
```

The output from running the test will read as the original requirements, providing you with not only automated tests but also a living document of the requirements in a business-readable format.

If you prefer to gather requirements as [User Stories](https://en.wikipedia.org/wiki/User_story) then you may prefer to take advantage of the `story()` wrapper for `describe()` instead.

```javascript
story("As a distribution manager, I want to know the volume of the box I need", function() {
    given("I have a width of 20
        And a height of 30
        And a depth of 40", function() {
        when("I run the calculation", function() {
              then("the result should be 24000", function() {
                  // call the method with the arguments and test the outcome
                  expect(myObject.myFunction(20,30,40)).toBe(24000);
              });
         });
    });
});
```

As `feature()`, `scenario()` and `story()` are wrappers for `describe()` you can intermix them so that your can create tests which read as the business requirements. As with `describe()`, they can be nested to build up blocks.