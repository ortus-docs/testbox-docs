# Custom Matchers

TestBox comes with a decent amount of matchers that cover what we believe are common scenarios. However, we recommend that you create custom matchers that meet your needs and criteria so that you can avoid duplication and have re-usability.

Every custom matcher is a function and must have the following signature, with `MyMatcher` being the name of your custom matcher function:

```javascript
boolean function MyMatcher( required expectation, args={} )
```

The matcher function receives the `expectation` object and a second argument which is a structure of all the arguments with which the matcher function was called with. It must then return a **true** or a **false** depending if it passes your criteria. It will most likely use the `expectation` object to retrieve the **actual** and **isNot** values. It can also set a custom failure message on the expectation object itself by using the `message` property of the `expectation` object.

```javascript
boolean function reallyFalse( expectation, args={} ){
     expectation.message = ( structKeyExists( args, "message" ) ? args.message : "[#expectation.actual#] is not really false" );
     if( expectation.isNot )
          return ( expectation.actual eq true );
     else
          return ( expectation.actual eq false );
     }
}
```

The next step is to tell TestBox about your matcher.

## Matcher Registration

You can register matcher functions in several ways within TestBox, but we always recommend that you register them inside of the `beforeAll()` or `beforeEach()` life-cycle method blocks for performance considerations and global availability.

### Inline matchers

You can pass a structure of key\/value pairs of the matchers you would like to register. The key is the name of the matcher function and the value is the closure function representation.

```javascript
function beforeAll(){

  addMatchers( {
       toBeAwesome : function( expectation, args={} ){ return expectation.actual gte 100; },
       toBeGreat : function( expectation, args={} ){ return expectation.actual gte 1000; },
       // please note I use positional values here, you can also use name-value arguements.
       toBeGreaterThan : function( expectation, args={} ){ return ( expectation.actual gt args[ 1 ]  ); }
  } );

}
```

After it is registered, then you can use it.

```javascript
it("A custom matcher", function(){
  expect( 100 ).toBeAwesome();
  expect( 5000 ).toBeGreat();
  expect( 10 ).toBeGreaterThan( 5 );
});
```

### CFC Matchers

You can also store a [plethora](http://en.wikipedia.org/wiki/Plethora) of matchers \(Yes, I said plethora\), in a CFC and register that as the matchers via its instantiation path. This provides much more flexibility and re-usability for your projects.

```javascript
addMatchers( "model.util.MyMatchers" );
```

You can also register a CFC instance:

```javascript
addMatchers( new models.util.MyMatchers() );
```

