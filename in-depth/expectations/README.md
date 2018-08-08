# Expectations

TestBox allows you to create BDD expectations with our expectations and matcher API DSL. You start by calling our `expect()` method, usually with an **actual** value you would like to test. You then concatenate the expectation of that actual value/function to a result or what we call a matcher. You can also concatenate matchers \(as of v2.1.0\) so you can provide multiple matching expectations to a single value.

```javascript
expect( 43 ).toBe( 42 );
expect( function(){ calculator.add(2,2); } ).toThrow();
```

