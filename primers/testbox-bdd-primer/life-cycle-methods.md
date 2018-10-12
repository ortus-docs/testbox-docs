# Life-Cycle Methods

## Before and After Specs

If you are familiar with xUnit style frameworks, the majority of them provide a way to execute functions before and after every single test case or spec in BDD. This is a great way to keep your tests DRY \(Do not repeat yourself\)! TestBox provides the `beforeEach()` and `afterEach()` methods that each take in a closure as their argument that receive the name of the spec that's about to be executed or just executed. As their names indicate, they execute before a spec and after a spec in a related `describe` block.

```javascript
describe("A spec (with setup and tear-down)", function() {

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
});
```

## Around Specs

The `aroundEach()` life-cycle method will completely wrap your spec in another closure. This is an elegant way for you to provide a complete around AOP advice to a specification. You can use it to surround the execution in transaction blocks, ORM rollbacks, logging, and so much more. This life-cycle method will decorate ALL specs within a single suite and any children suites. The method signature is below:

```javascript
aroundEach( function( spec, suite ){
     // execute the spec
     arguments.spec.body();
} );
```

The method receives a structure of data representing the spec. This contains the following elements:

* **body** : The actual closure for the spec that you will use to execute within it.
* **labels** : The labels used in the spec
* **name** : The name of the spec
* **order** : The order of execution of the spec
* **skip** : The skip flag or closure that determines if the spec runs

The method also receives a structure of metadata about the suite this spec is contained in. It has information about life-cycle closures, async information, names, parents, etc. Here is a very practical example of creating and around each closure to provide rollbacks for specs:

```javascript
aroundEach( function( spec, suite ){

     transaction action="begin"{
          try{/
               // execute the spec
               arguments.spec.body();
          } catch( Any e ){
               rethrow;
          } finally{
               transaction action="rollback";
          }
     }
} );
```

This simple around each life-cycle closure will rollback ALL my spec's executions even if they throw exceptions.

### Annotations

TestBox since v2.3.0 also allows you to declare life-cycle methods via annotations. Please see our [Annotations](../../in-depth/life-cycle-methods/annotations.md) section for more information.

## Lifecycle Nesting Order

When you use `beforeEach()`, `afterEach()`, and `aroundEach()` at the same time, there is a specific order they fire in.  For a given describe block, they will fire in this order.  Remember, `aroundEach()` is split into two parts-- the half of the method before you call `spec.body()` and the second half of the method.

1. beforeEach
2. aroundEach \(first half\)
3. it\(\) \(the `spec.body()` call\)
4. aroundEach \(second half\)
5. afterEach\(\)

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

If there are more than one `it()` blocks, the process repeats for each one.  Steps 1, 2, 4, 5 will wrap every single `it()`.  

When you nest more than one describe block inside the other, the before/around/after order is the same but drills down to the innermost describe and then bubbles back up.  That means the outermost `beforeEach()` starts and we end on the outermost `afterEach()`.   

Here's what an example flow would look like that had before/after/around specified in two levels of describes with a single `it()` in the inner most describe. 

1. Outermost `beforeEach()` call
2. Innermost `beforeEach()` call
3. Outermost `aroundEach()` call \(first half\)
4. Innermost `aroundEach()` call \(first half\)
5. The `it()` block
6. Innermost `aroundEach()` calls \(second half\)
7. Outermost `aroundEach()` call \(second half\)
8. Innermost `afterEach()` call
9. Outermost `afterEach()` call

This works regardless of the number of levels and can obviously have many permutations, but the basic order is still the same.  Before/around/after and starting at the outside working in, and back out again.  This process happens for every single spec or `it()` block.  This is as opposed to the `beforeAll()` and `afterAll()` method which only run once for the entire CFC regardless of how many specs there are.

