# Run Listeners

Every `run` and `runRaw` methods now accept a `callbacks` argument, which can be a CFC with the right listener methods or a struct with the right closure methods.

The available callbacks are
function onBundleStart( target, testResults )
function onBundleEnd( target, testResults )

function onSuiteStart( target, testResults, suite )
function onSuiteEnd( target, testResults, suite )

function onSpecStart( target, testResults, suite, spec )
function onSpecEnd( target, testResults, suite, spec )
