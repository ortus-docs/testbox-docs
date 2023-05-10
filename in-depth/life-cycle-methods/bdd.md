# BDD

## Global Callbacks

Global callbacks affect the execution of the entire test bundle CFC and all of its suites and specs.

### beforeAll()

Executes **once** before all specs for the **entire** test bundle CFC. A great place to initialize the environment the bundle needs for testing.

```javascript
component{

	function beforeAll(){
		ORMSessionClear();
		structClear( request );  
		
		// Prepare jwt driver to use cache instead of db for easier mocking
		variables.jwt.getSettings().jwt.tokenStorage.driver = "cachebox";
		variables.jwt.getSettings().jwt.tokenStorage.properties = { cacheName : "default" };

		// Logout just in case
		variables.securityService.logout(); 
	}

}
```

### afterAll()

Executes **once** after all specs for the **entire** test bundle CFC. A great place to teardown the environment the bundle needed for testing.

```javascript
component{

	function afterAll(){
		variables.securityService.logout();
		directoryDelete( "/tests/tmp", true );
	}

}
```

### run( testResults, testBox )

Executes **once** so it can capture all your `describe` and `it` blocks so they can be executed by a TestBox runner.&#x20;

```javascript
function run( testResults, testbox ){

    describe("A Spec", function(){
    
    });

}
```

{% hint style="info" %}
You can find the API docs for `testbox` and the `testResults` arguments here: [https://s3.amazonaws.com/apidocs.ortussolutions.com/testbox/current/](https://s3.amazonaws.com/apidocs.ortussolutions.com/testbox/current/)
{% endhint %}

## Suite Callbacks

The following callbacks influence the execution of specification methods: `it(), then()`.  The great flexibility of the BDD approach is that it allows you to nest `describe`, `feature`, `story`, `given`, `scenario`, `when` suite blocks to create very human readable and organized documentation for your tests. Each suite block can have its own life-cycle methods as well. Not only that, if they are nested, TestBox will walk the tree and call each `beforeEach()` and `afterEach()` in the order you declare them.

{% hint style="success" %}
TestBox will walk down the tree (from the outermost suite) for `beforeEach()` operations and out of the tree (from the innermost suite) for `afterEach()` operations.
{% endhint %}

### beforeEach( body, data )

Executes before **every** single spec in a single suite block and receives the currently executing spec and any [data you want to bind the specification](../../primers/testbox-bdd-primer/specs.md#spec-data-binding) with.  The `body` is a closure/lambda that will fire and the `data` argument is a way to [bind the life-cycle method ](bdd.md#life-cycle-data-binding)with a struct of data that can flow down to specs.

The `body` closure will receive have the following signature:

```javascript
function( currentSpec, data ){

}

(currentSpec, data ) => {}
```

### afterEach( body, data )

Executes after **every** single spec in a single suite block and receives the currently executing spec and any [data you want to bind the specification](../../primers/testbox-bdd-primer/specs.md#spec-data-binding) with.  The `body` is a closure/lambda that will fire and the `data` argument is a way to [bind the life-cycle method ](bdd.md#life-cycle-data-binding)with a struct of data that can flow down to specs.

The `body` closure will receive have the following signature:

```javascript
function( currentSpec, data ){

}

(currentSpec, data ) => {}
```

Here are some examples:

```javascript
component{

     function run( testResults, testBox ){
          describe("A Spec", function(){

               beforeEach( function( currentSpec, data ){
                    // before each spec in this suite
               });

               afterEach( function( currentSpec, data ){
                    // after each spec in this suite
               });

               describe("A nested suite", function(){

                    // my parent's aroundEach()

                    beforeEach( ( currentSpec, data ) => {
                         // before each spec in this suite + my parent's beforeEach()
                    });

                    afterEach( ( currentSpec, data ) => {
                         // after each spec in this suite + my parent's afterEach()
                    });

                });

          });

          describe("A second spec", function(){

               beforeEach( function( currentSpec, data ){
                    // before each spec in this suite, separate from the two other ones
               });

               afterEach( function( currentSpec, data ){
                    // after each spec in this suite, separate from the two other ones
               });

          });
     }
}
```

### aroundEach( body, data )

Executes **around** the executing spec so you can provide code that will surround the execution of the spec.  It's like combining `before` and `after` in a single operation.  The `body` is a closure/lambda that will fire and the `data` argument is a way to [bind the life-cycle method ](bdd.md#life-cycle-data-binding)with a struct of data that can flow down to specs.  This is the only way you can use CFML constructs that wrap around code like: try/catch, transaction, for, while, etc.

The `body` closure will receive have the following signature:

```javascript
function( spec, suite, data ){

}

(spec, suite, data) => {}
```

The `spec` is the currently executing specification, the `suite` is the suite this life-cycle is embedded in and `data` is the data binding, if any.

Here is an example:

```javascript
component{

     function run( testResults, testBox ){
          describe("A Spec", function(){

               aroundEach( function( spec, suite, data ){
                    ormClearSession();
               			ormCloseSession();
               			try {
               					// Make sure we always rollback
               					transaction {
               						arguments.spec.body();
               					}
               			} catch ( any e ) {
               					transactionRollback();
               					rethrow;
               			}
               });

               describe("A nested suite", function(){

                    // my parent's aroundEach()

                    beforeEach( function( currentSpec, data ){
                         // before each spec in this suite + my parent's beforeEach()
                    });

                    afterEach( function( currentSpec, data ){
                         // after each spec in this suite + my parent's afterEach()
                    });

                });

          });
     }
}
```

## Life-Cycle Data Binding

You can pass in an argument called `data` , which is a `struct` of dynamic data, to all life-cycle methods.  This is useful when creating dynamic suites and specifications.  This `data` will then be passed into the executing body for each life-cycle method for you.

```javascript
beforeEach( 
    data = { mydata="luis" }, 
    body = function( currentSpec, data ){
        // The arguments.data is binded via the `data` snapshot above.
        data.myData == "luis";
    }
);
```

Here is a typical example:

```javascript
describe( "Ability to bind data to life-cycle methods", function(){

    var data = [
        "spec1",
        "spec2"
    ];

    for( var thisData in data ){
        describe( "Trying #thisData#", function(){

            beforeEach( 
                data : { myData = thisData }, 
                body : function( currentSpec, data ){
                    targetData = arguments.data.myData;
            });

            it( 
                title : "should account for life-cycle data binding", 
                data  : { myData = thisData },
                body  : function( data ){
                    expect( targetData ).toBe( data.mydata );
                }
            );

            afterEach( 
                data : { myData = thisData }, 
                body : function( currentSpec, data ){
                    targetData = arguments.data.myData;
            });
        });
    }

    for( var thisData in data ){

        describe( "Trying around life-cycles with #thisData#", function(){

            aroundEach( 
                data : { myData = thisData }, 
                body : function( spec, suite, data ){
                    targetData = arguments.data.myData;
                    arguments.spec.body( data=arguments.spec.data );
            });

            it( 
                title : "should account for life-cycle data binding", 
                data  : { myData = thisData },
                body  : function( data ){
                    expect(    targetData ).toBe( data.mydata );
            });

        });

    }
});
```
