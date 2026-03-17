---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-bdd-primer/specs-and-suite-labels
---

# Specs and Suite Labels

Specs and suites can be tagged with TestBox labels. Labels allow you to further categorize different specs or suites so that when a runner executes with `labels` attached, only those specs and suites will be executed; the rest will be skipped. You can alternatively choose to skip specific labels when a runner executes with `excludes` attached.

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

## Direct Suite Name Matching

When using the `testSuites` filter (via a runner, the CLI, or `TestBox` directly), TestBox performs **direct suite name matching** at any nesting depth. This means if a suite's name exactly matches the filter value it will always be included — regardless of where it sits in the nesting hierarchy.

```javascript
// This suite will always be included when testSuites="My Integration Suite"
// even if it is nested 3 levels deep
describe("My Integration Suite", function() {
     it("runs when targeted by name", function() {
          expect( true ).toBeTrue();
     });
});
```

Targeting by name from the various runners:

```bash
# BoxLang CLI runner
./testbox/run --filter-suites="My Integration Suite"

# CommandBox CLI runner
testbox run testSuites="My Integration Suite"
```

```javascript
// Programmatically via TestBox
new testbox.system.TestBox(
    bundles    = "tests.specs",
    testSuites = "My Integration Suite"
).run();
```
