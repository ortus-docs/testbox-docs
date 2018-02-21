# Asynchronous Testing

As you can see from our arguments for a test suite, you can pass an `asyncAll` argument to the `describe()` blocks that will allow TestBox to execute all specs in separate threads for you concurrently.

```javascript
describe(title="A spec (with setup and tear-down)", asyncAll=true, body=function() {

     beforeEach(function() {
          coldbox = 22;
          application.wirebox = new coldbox.system.ioc.Injector();
     });

     afterEach(function() {
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

> **Caution** Once you delve into the asynchronous world you will have to make sure your tests are also thread safe (var-scoped) and provide any necessary locking.
