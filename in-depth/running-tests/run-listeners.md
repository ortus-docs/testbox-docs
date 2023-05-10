# Run Listeners

Every `run` and `runRaw` methods now accept a `callbacks` argument, which can be a CFC with the right listener methods or a struct with the right closure methods. This will allow you to listen to the testing progress and get information about it. This way you can build informative reports or progress bars.

The available callbacks are:

```javascript
// Called at the beginning of a test bundle cycle
function onBundleStart( target, testResults )
// Called at the end of the bundle testing cycle
function onBundleEnd( target, testResults )

// Called anytime a new suite is about to be tested
function onSuiteStart( target, testResults, suite )
// Called after any suite has finalized testing
function onSuiteEnd( target, testResults, suite )

// Called anytime a new spec is about to be tested
function onSpecStart( target, testResults, suite, spec )
// Called after any spec has finalized testing
function onSpecEnd( target, testResults, suite, spec )
```
