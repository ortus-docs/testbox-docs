# Custom Assertions

TestBox comes with a plethora of assertions that cover what we believe are common scenarios. However, we recommend that you create custom assertions that meet your needs and criteria so that you can avoid duplication and have re-usability. A custom assertion function can receive any amount of arguments but it must use the `fail()` method in order to fail an assertion or just return true or void for passing.

Here is an example:

```javascript
function isAwesome( required expected ){
     return ( arguments.expected == "TestBox" ? fail( 'TestBox is always awesome' ) : true );
}
```
##Assertion Registration

You can register assertion functions in several ways within TestBox, but we always recommend that you register them inside of the `beforeTests()` or `setup()` life-cycle method blocks, so they are only inserted once.

Inline Assertions You can pass a structure of key/value pairs of the assertions you would like to register. The key is the name of the assertion function and the value is the closure function representation.

```javascript
function beforeTests(){

     addAssertions({
          isAwesome = function( required expected ){
               return ( arguments.expected == "TestBox" ? true : fail( 'not TestBox' ) );
          },
          isNotAwesome = function( required expected ){
               return ( arguments.expected == "TestBox" ? fail( 'TestBox is always awesome' ) : true );
          }
     });

}
```

After it is registered, then you can just use it out of the `$assert` object it got mixed into.

```javascript
function testAwesomenewss(){
  $assert.isAwesome( 'TestBox' );
}
```

##CFC Assertions 
You can also store a [plethora](http://en.wikipedia.org/wiki/Plethora) of assertions (Yes, I said plethora), in a CFC and register that as the assertions via its instantiation path. This provides much more flexibility and reusability for your projects.

```javascript
addAssertions( "model.util.MyAssertions" );
```

You can also register more than 1 CFC by using a list or an array:

```javascript
addAssertions( "model.util.MyAssertions, model.util.RegexAssertions" );
addAssertions( [ "model.util.MyAssertions" , "model.util.RegexAssertions" ] );
```

Here is the custom assertions CFC source:

```javascript
component{

     function assertIsAwesome( expected, actual ){
          return ( expected eq actual ? true : false );
     }

     function assertIsFunky( actual ){
          return ( actual gte 100 ? true : false );
     }

}
```


