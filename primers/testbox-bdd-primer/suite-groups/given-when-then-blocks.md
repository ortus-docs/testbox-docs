# Given-When-Then Blocks

[Given-When-Then](http://martinfowler.com/bliki/GivenWhenThen.html) is a style of writing tests where you describe the state of the code you want to test \(`Given`\), the behavior you want to test \(`When`\) and the expected outcome \(`Then`\). \(See [Specification By Example](http://martinfowler.com/bliki/SpecificationByExample.html)\)

Testbox supports the use of function names `given()` and `when()` in-place of `describe()` function calls. The `then()` function call is an alternative for `it()` function calls. The advantage of this style of [behavioural specifications](https://en.wikipedia.org/wiki/Behavior-driven_development#Behavioural_specifications) is that you can gather your requirements and write your tests in a common language that can be understood by developers and stake-holders alike. This common language format is often referred to as the [Gherkin](https://github.com/cucumber/cucumber/wiki/Gherkin) language; using it we can gather and document the requirements as:

```text
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
