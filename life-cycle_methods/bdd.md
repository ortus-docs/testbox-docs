# BDD

* `beforeAll()` - Executes once before all specs for the entire test bundle CFC
* `afterAll()` - Executes once after all specs complete in the test bundle CFC
* `run( testResults, TestBox )` - Executes once so it can capture all your describe and it blocks
* `beforeEach( body, data )` - Executes before every single spec in a single describe block and receives the currently executing spec.
* `afterEach( body, data )` - Executes after every single spec in a single describe block and receives the currently executing spec.
* `aroundEach( body, data )` - Executes around the executing spec so you can provide code surrouding the spec.

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
               
                    // my parent's aroundEach()

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

## Life-Cycle Data Binding

You can pass in an argument called `data` which is a `struct` of dynamic data to pass into the life-cycle method.  You can then pickup this data in the closure for the life-cycle. 

```
beforeEach( 
	data = { mydata="luis" }, 
	body = function( currentSpec, data ){
		// The arguments.data is binded via the `data` snapshot above.
		data.myData == "luis";
	}
);


Here is a typical example:

```js
describe( "Ability to bind data to life-cycle methods", function(){
			
	var data = [
		"spec1",
		"spec2"
	];

	for( var thisData in data ){
		describe( "Trying #thisData#", function(){
			
			beforeEach( data={ myData = thisData }, body=function( currentSpec, data ){
				targetData = arguments.data.myData;
			});
			
			it( title="should account for life-cycle data binding", 
				data={ myData = thisData},
				body=function( data ){
					expect(	targetData ).toBe( data.mydata );
				}
			);

			afterEach( data={ myData = thisData }, body=function( currentSpec, data ){
				targetData = arguments.data.myData;
			});
		});
	}

	for( var thisData in data ){

		describe( "Trying around life-cycles with #thisData#", function(){
			
			aroundEach( data={ myData = thisData }, body = function( spec, suite, data ){
				targetData = arguments.data.myData;
				arguments.spec.body( data=arguments.spec.data );
			});

			it( title="should account for life-cycle data binding", 
				data={ myData = thisData },
				body=function( data ){
					expect(	targetData ).toBe( data.mydata );
				}
			);
		
		});

	}
});
```



