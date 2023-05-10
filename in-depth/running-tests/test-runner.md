# Test Runner

If you make your test bundle CFC inherit from our `testbox.system.BaseSpec` class, you will be able to execute the CFC directly via the URL:

```javascript
http://localhost/test/MyTest.cfc?method=runRemote
```

You can also pass the following arguments to the method via the URL:

* `testSuites` : A list or array of suite names that are the ones that will be executed ONLY!
* `testSpecs` : A list or array of test names that are the ones that will be executed ONLY!
* `reporter` : The type of reporter to run the test with

```javascript
http://localhost/test/MyTest.cfc?method=runRemote&reporter=json
```
