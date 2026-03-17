---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-bdd-primer/life-cycle-methods
---

# Life-Cycle Methods

## Global Callbacks <a href="#global-callbacks" id="global-callbacks"></a>

Global callbacks affect the execution of the entire test bundle CFC and all of its suites and specs.

### beforeAll() <a href="#beforeall" id="beforeall"></a>

Executes **once** before all specs for the **entire** test bundle CFC. A great place to initialize the environment the bundle needs for testing.

```cfscript
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

### afterAll() <a href="#afterall" id="afterall"></a>

Executes **once** after all specs for the **entire** test bundle CFC. A great place to teardown the environment the bundle needed for testing.

Copy

```cfscript
component{

	function afterAll(){
		variables.securityService.logout();
		directoryDelete( "/tests/tmp", true );
	}

}
```

### run( testResults, testBox ) <a href="#run-testresults-testbox" id="run-testresults-testbox"></a>

Executes **once** so it can capture all your `describe` and `it` blocks so they can be executed by a TestBox runner.

```cfscript
function run( testResults, testbox ){

    describe("A Spec", function(){
    
    });

}
```

{% hint style="info" %}
You can find the API docs for `testbox` and the `testResults` arguments here: [https://s3.amazonaws.com/apidocs.ortussolutions.com/testbox/current/](https://s3.amazonaws.com/apidocs.ortussolutions.com/testbox/current/)
{% endhint %}

## Suite CallBacks

The following callbacks influence the execution of specification methods: `it(), then()`. The great flexibility of the BDD approach is that it allows you to nest `describe`, `feature`, `story`, `given`, `scenario`, `when` suite blocks to create very human readable and organized documentation for your tests. Each suite block can have its own life-cycle methods as well. Not only that, if they are nested, TestBox will walk the tree and call each `beforeEach()` and `afterEach()` in the order you declare them.

{% hint style="info" %}
TestBox will walk down the tree (from the outermost suite) for `beforeEach()` operations and out of the tree (from the innermost suite) for `afterEach()` operations.
{% endhint %}

### beforeEach( body, data ) <a href="#beforeeach-body-data" id="beforeeach-body-data"></a>

Executes before **every** single spec in a single suite block and receives the currently executing spec and any [data you want to bind the specification](https://testbox.ortusbooks.com/primers/testbox-bdd-primer/specs#spec-data-binding) with. The `body` is a closure/lambda that will fire and the `data` argument is a way to [bind the life-cycle method ](https://testbox.ortusbooks.com/in-depth/life-cycle-methods/bdd#life-cycle-data-binding)with a struct of data that can flow down to specs.

The `body` closure will receive have the following signature:

```cfscript
function( currentSpec, data ){

}

(currentSpec, data ) => {}
```

### afterEach( body, data ) <a href="#aftereach-body-data" id="aftereach-body-data"></a>

Executes after **every** single spec in a single suite block and receives the currently executing spec and any [data you want to bind the specification](https://testbox.ortusbooks.com/primers/testbox-bdd-primer/specs#spec-data-binding) with. The `body` is a closure/lambda that will fire and the `data` argument is a way to [bind the life-cycle method ](https://testbox.ortusbooks.com/in-depth/life-cycle-methods/bdd#life-cycle-data-binding)with a struct of data that can flow down to specs.

The `body` closure will receive have the following signature:

```cfscript
function( currentSpec, data ){

}

(currentSpec, data ) => {}
```

Here are some examples:

```cfscript
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

### aroundEach( body, data ) <a href="#aroundeach-body-data" id="aroundeach-body-data"></a>

Executes **around** the executing spec so you can provide code that will surround the execution of the spec. It's like combining `before` and `after` in a single operation. The `body` is a closure/lambda that will fire and the `data` argument is a way to [bind the life-cycle method ](https://testbox.ortusbooks.com/in-depth/life-cycle-methods/bdd#life-cycle-data-binding)with a struct of data that can flow down to specs. This is the only way you can use CFML constructs that wrap around code like: try/catch, transaction, for, while, etc.

The `body` closure will receive have the following signature:

```cfscript
function( spec, suite, data ){

}

(spec, suite, data) => {}
```

The `spec` is the currently executing specification, the `suite` is the suite this life-cycle is embedded in and `data` is the data binding, if any.

Here is an example:

```cfscript
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



## Lifecycle Nesting Order

When you use `beforeEach()`, `afterEach()`, and `aroundEach()` at the same time, there is a specific order they fire in.  For a given describe block, they will fire in this order.  Remember, `aroundEach()` is split into two parts-- the half of the method before you call `spec.body()` and the second half of the method.

1. beforeEach
2. aroundEach (first half)
3. it() (the `spec.body()` call)
4. aroundEach (second half)
5. afterEach()

Here's an example:

```javascript
describe( 'my describe', function(){
	
    beforeEach( function( currentSpec ){
        // I run first
    } );
    	 
    aroundEach( function( spec, suite ){
        // I run second
        arguments.spec.body();
        // I run fourth
    });
    
    afterEach( function( currentSpec ){
        // I run fifth
    } );
    
    it( 'my it', function(){
        // I run third
    } );
    
} );
```

If there are more than one `it()` blocks, the process repeats for each one.  Steps 1, 2, 4, 5 will wrap every single `it()`. &#x20;

When you nest more than one describe block inside the other, the before/around/after order is the same but drills down to the innermost describe and then bubbles back up.  That means the outermost `beforeEach()` starts and we end on the outermost `afterEach()`.  &#x20;

Here's what an example flow would look like that had before/after/around specified in two levels of describes with a single `it()` in the inner most describe.&#x20;

1. Outermost `beforeEach()` call
2. Innermost `beforeEach()` call
3. Outermost `aroundEach()` call (first half)
4. Innermost `aroundEach()` call (first half)
5. The `it()` block
6. Innermost `aroundEach()` calls (second half)
7. Outermost `aroundEach()` call (second half)
8. Innermost `afterEach()` call
9. Outermost `afterEach()` call

This works regardless of the number of levels and can obviously have many permutations, but the basic order is still the same.  Before/around/after and starting at the outside working in, and back out again.  This process happens for every single spec or `it()` block.  This is as opposed to the `beforeAll()` and `afterAll()` method which only run once for the entire CFC regardless of how many specs there are.
