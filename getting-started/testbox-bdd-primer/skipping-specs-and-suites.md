---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-bdd-primer/skipping-specs-and-suites
---

# Skipping Specs and Suites

Specs and suites can be skipped from execution by prefixing certain functions with the letter `x` or by using the skip argument in each of them or by using the `skip( message, detail )` function. The reporters will show that these suites or specs were skipped from execution. The functions you can prefix are:

* `it()`
* `describe()`
* `story()`
* `given()`
* `when()`
* `then()`
* `feature()`



Here are some examples:

```javascript
xdescribe("A spec", function() {
     it("was just skipped, so I will never execute", ()=>{
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
     });
});

describe("A spec", function() {
     it("is just a closure, so it can contain any code", ()=>{
          coldbox = 0;
          coldbox++;
          expect( coldbox ).toBe( 1 );
     });

     xit("can have more than one expectation, but I am skipped", ()=> {
          coldbox = 0;
          coldbox++;
          expect( coldbox ).toBe( 1 );
          expect( coldbox ).toBeTrue();
     });
     
     it( "can only run on lucee", ()=>{
          if( !server.keyExists( "lucee" ) ){
               skip( "Only for lucee" );
          }
     } );
});
```

## Skipping An Entire Class

As of TestBox 7.1, a BDD test class can carry a class-level `skip` annotation, so the whole class is skipped without prefixing every `describe()` or editing your runner filters.

```javascript
/**
 * @skip
 */
component extends="testbox.system.BaseSpec" {

    function run(){
        describe( "Payment gateway", function(){
            // none of this runs while @skip is present
        } );
    }

}
```

You can supply a reason, which shows up in the reporters:

```javascript
/**
 * @skip Waiting on the sandbox credentials
 */
```

Skipped classes are reported as skipped rather than silently dropped, so your totals stay honest and the class does not quietly rot.

{% hint style="info" %}
This is the class-level equivalent of `xdescribe()`. Use it when an entire bundle is blocked on something external, and prefer the [skip argument](#skip-argument) below when the decision depends on the engine or on runtime state.
{% endhint %}

## Skip Argument

The `skip` argument can be a boolean value or a closure. If the value is **true** then the suite or spec is skipped. If the return value of the closure is **true** then the suite or spec is skipped. Using the closure approach allows you to dynamically at runtime figure out if the desired spec or suite is skipped. This is such a great way to prepare tests for different CFML engines.

```javascript
describe(title="A railo suite", body=function() {
     it("can be expected to run", function() {
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
     });

     it(title="can have more than one expectation and another skip closure", body=function() {
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
          expect( coldbox ).toBeTrue();

     },skip=function(){
          return false;
     });

},skip=function(){
     return !structKeyExists( server, "railo" );
});
```

## Skip Method

You can now use the `skip( message, dteail )` method to skip any spec or suite a-la-carte instead of as an argument to the function definitions.  This lets you programmatically skip certain specs and suites and pass a nice message.

```cfscript
it( "can do something", () => {
    ...
    if( condition ){
        skip( "Condition is true, skipping spec" )
    }
    ...
} )
```
