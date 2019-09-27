# Focused Specs and Suites

Specs and suites can be focused so ONLY those suites and specs execute.  You will do this by prefixing certain functions with the letter `f` or by using the `focused` argument in each of them. The reporters will show that these suites or specs where execute ONLY The functions you can prefix are:

* `it()`
* `describe()`
* `story()`
* `given()`
* `when()`
* `then()`
* `feature()`

```javascript
fstory( "A spec", function() {
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

     fit("can have more than one expectation, but I am skipped", function() {
          coldbox = 0;
          coldbox++;

          expect( coldbox ).toBe( 1 );
          expect( coldbox ).toBeTrue();
     });
});
```

{% hint style="warning" %}
Please note that if a suite is focused, then all of its children will execute.
{% endhint %}

