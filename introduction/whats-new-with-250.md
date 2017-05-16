# What's New With 2.5.0

TestBox 2.5.0 is a minor release with some great new functionality and tons of fixes.  You can find the release notes here and the major updates for this release. One of the biggest features for TestBox that was not part of TestBox, was the addition of TestBox Watchers to CommandBox.

## TestBox Watchers

CommandBox CLI has added watching capabilities to the `testbox` namespace.  Which means you can now run the following command in the root of your project: `testbox watch` and it will monitor all your CFCs for changes.  If a change is detected, then it will fire the new `testbox run` command with our CLI reporter.

## Life-Cycle Data Binding
We had introduced spec data binding in TestBox for creating dynamic specs and passing dynamic data into them.  We have extended this feature into life-cycle methods:

* `beforeEach()`
* `afterEach()`
* `aroundEach()`



## Release Notes    

### Bugs
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-177'>TESTBOX-177</a>] - Simple reporter broken on Adobe servers
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-180'>TESTBOX-180</a>] - HTML Runner links should include directory param
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-181'>TESTBOX-181</a>] - AsyncAll Errroring on xUnit runner
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-183'>TESTBOX-183</a>] - the &#39;type&#39; argument was being ignored if the &#39;regex&#39; argument was provided and matched the exception data          

### Improvements
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-178'>TESTBOX-178</a>] - Remove thread scope from debug stream
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-184'>TESTBOX-184</a>] - Passing Data to Dynamic Tests via life-cycle methods: around, after, before
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-185'>TESTBOX-185</a>] - Preserve whitespace in HTML (simple) Reporter
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-186'>TESTBOX-186</a>] - Include TestBox version in TestResult memento
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-187'>TESTBOX-187</a>] - Let travis handle upload of api docs and snapshot binaries
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-188'>TESTBOX-188</a>] - Aggregate suite stats from nested suties
