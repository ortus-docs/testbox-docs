# Not Operator

You can prefix your expectation with the `not` operator to easily cause negative expectations for **any** matcher. When you read the [API Docs](http://apidocs.ortussolutions.com/testbox/2.1.0/index.html) or the source, you will find nowhere the not methods. This is because we do this dynamically by convention.

```javascript
expect( actual )
     .notToBe( 4 )
     .notToBeTrue();
     .notToBeFalse();
```

