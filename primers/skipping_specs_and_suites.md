# Skipping Specs and Suites

Specs and suites can be skipped from execution by using the `xdescribe()` and `xit()` functions or by using the skip argument in each of them. The reporters will show that these suites or specs where skipped from execution.

```javascript
xdescribe("A spec", function() {
     it("was just skipped, so I will never execute", function() {
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
     });
});

describe("A spec", function() {
     it("is just a closure, so it can contain any code", function() {
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
     });

     xit("can have more than one expectation, but I am skipped", function() {
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
          expect( coldbox ).toBeTrue();
     });
});
```

<h3 style="color:grey">Skip Argument</h3>

The skip argument can be a boolean value or a closure. If the value is true then the suite or spec is skipped. If the return value of the closure is true then the suite or spec is skipped. Using the closure approach allows you to dynamically at runtime figure out if the desired spec or suite is skipped. This is such a great way to prepare tests for different CFML engines.

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


