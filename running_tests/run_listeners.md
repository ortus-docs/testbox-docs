# Run Listeners

Every `run` and `runRaw` methods now accept a `callbacks` argument, which can be a CFC with the right listener methods or a struct with the right closure methods.  This will allow you to listen to the testing progress and get information about it. This way you can build informative reports or progress bars.

The available callbacks are:
```javascript
function onBundleStart( target, testResults )
function onBundleEnd( target, testResults )

function onSuiteStart( target, testResults, suite )
function onSuiteEnd( target, testResults, suite )

function onSpecStart( target, testResults, suite, spec )
function onSpecEnd( target, testResults, suite, spec )
```