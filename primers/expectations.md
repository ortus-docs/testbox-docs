# Expectations

Expectations are self-concatenated strings that evaluate an **actual** value to an **expected** value or condition. These are initiated by the global TestBox method called `expect()` which takes in a value called the **actual** value. It is concatenated in our expectations DSL with a matcher function that will most likely take in an expected value or condition to test. You can also concatenate the matchers and do multiple evaluations on a single actual value.

## Matchers

Each matcher implements a comparison or evaluation of the **actual** value and an **expected** value or condition. It is responsible for either passing or failing this evaluation and reporting it to TestBox. Each matcher also has a negative counterpart assertion by just prefixing the call to the matcher with a `not` expression.

```javascript
function run(){

     describe("The 'toBe' matcher evaluates equality", function(){
          it("and has a positive case", function(){
               expect( true ).toBe( true );
          });

          it("and has a negative case", function(){
               expect( false ).notToBe( true );
          });
     });

}
```

### Included Matchers

TestBox has a plethora (That's Right! I said [Plethora](http://www.merriam-webster.com/dictionary/plethora)) of matchers that are included in TestBox. The best way to see all the latest matchers is to visit our [API](http://apidocs.ortussolutions.com/testbox/current) and digest the `testbox.system.Expectation` class. There is also the ability to register and write custom matchers in TestBox via our `addMatchers()` function at runtime.


```javascript
describe("Some Included TestBox Matchers:", function() {

     describe("The 'toBe' matcher", function() {
          it("works for simple values", function() {
               coldbox = 1;
               expect( coldbox )
                    .toBe( 1 )
                    .notToBe( 5 );
          });

          it("works for complex values too (arrays,structs,queries,objects)", function() {
               expect( [1,2] ).toBe( [1,2] );
               expect( { name="luis", awesome=true} ).toBe( { awesome=true, name="luis" } );
               expect( this ).toBe( this );
               expect( queryNew("") ).toBe( queryNew("") );
          });
     });

     it("The 'toBeWithCase' matches strings with case equality", function() {
          expect( 'hello' )
               .toBeWithCase( 'hello' )
               .notToBeWithCase( 'HELLO' );
     });

     it("The 'toBeTrue' and 'toBeFalse' matchers are used for boolean operations", function() {
          coldbox_rocks = true;
          expect( coldbox_rocks ).toBeTrue();
          expect( !coldbox_rocks ).toBeFalse();
     });

     it("The 'toBeNull' expects null values", function() {
          foo = "bar";
          expect( javaCast("null", "") ).toBeNull();
          expect( foo ).notToBeNull();
     });

     it("The 'toBeInstanceOf' expects the object to be of the same class or inheritance or implementation", function() {
          expect( new coldbox.system.Assertions ).toBeInstanceOf( 'coldbox.system.Assertions' );
          expect( this ).notToBeInstanceOf( 'coldbox.system.MockBox' );
     });

     it("The 'toMatch' matcher is for regular expressions with case sensitivity", function() {
          message = 'foo man choo';

          expect( message )
               .toMatch( '^foo' )
               .toMatch( '(man)' )
               .notToMatch( 'superman' );
     });

     it("The 'toMatchNoCase' matcher is for regular expressions with no case sensitivity", function() {
          message = 'foo man choo';

          expect( message )
               .toMatch( '^FOO' )
               .toMatch( '(MAN)' )
               .notToMatch( 'SuperMan' );
     });

     describe("The 'toBeTypeOf' matcher evaluates using the CF isValid() function", function() {
          it("works with direct calls", function() {
               expect( [1,2] ).toBeTypeOf( 'Array' );
               expect( { name="luis", awesome=true} ).toBeTypeOf( 'struct' );
               expect( this ).toBeTypeOf( 'component' );
               expect( '03/01/1990' ).toBeTypeOf( 'usdate' );
          });

          it("works with dynamic calls as well", function() {
               expect( [1,2] ).toBeArray();
               expect( { name="luis", awesome=true} ).toBeStruct();
               expect( this ).toBeComponent();
               expect( '03/01/1990' ).toBeUsDate();
          });
     });

     it("The 'toBeEmpty' checks emptyness of simple or complex values", function() {
          expect( [] ).toBeEmpty();
          expect( { name="luis", awesome=true} ).notToBeEmpty();
          expect( '' ).toBeEmpty();
     });

     it("The 'toHaveLength' checks size of simple or complex values", function() {
          expect( [] ).toHaveLength( 0 );
          expect( { name="luis", awesome=true} ).toHaveLength( 2 );
          expect( 'Hello' ).toHaveLength( 5 );
     });

     it("The 'toHaveKey' checks for existence of keys in structs", function() {
          expect( { name="luis", awesome=true} ).toHaveKey( 'name' );
     });

     it("The 'toHaveDeepKey' checks for existence of keys anywhere in structs", function() {
          expect( { name="luis", { age=35, awesome=true } } ).toHaveKey( 'age' );
     });

     it("The 'toThrow' checks if the actual call fails", function() {
          expect( function(){
               new calculator().divide( 40, 0 );
          }).toThrow();

          expect( function(){
               new calculator().divide( 40, 0 );
          }).toThrow( regex="zero" );
     });

});
```

### Custom Matchers

You can also build and register custom matchers. Please visit the Custom Matchers chapter to read more about custom matchers.

