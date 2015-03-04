# Specs and Suite Labels
Specs and suites can be tagged with TestBox labels. Labels allows you to further categorize different specs or suites so that when a runner executes with labels attached, only those specs and suites will be executed, the rest will be skipped.

```javascript
describe(title="A spec", labels="stg,railo", body=function() {
     it("executes if its in staging or in railo", function() {
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

     it(title="can have more than one expectation and labels", labels="dev,stg,qa,shopping", body=function() {
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
          expect( coldbox ).toBeTrue();
     });
});
```
