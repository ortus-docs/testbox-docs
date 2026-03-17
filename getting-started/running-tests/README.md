---
description: Test All Things!
icon: magnifying-glass-play
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/running-tests
---

# Running Tests

TestBox tests can be run from different sources from what we call **Runners.** These can be from different sources:

* CLI
  * TestBox CLI (Powered by CommandBox)
  * BoxLang Scripts
  * NodeJS
* Web Server / Browser
  * Web Runner
  * TestBundle Directly
  * **TestBox RUN IDE** (BoxLang-native browser IDE) 🆕
* Streaming (Real-Time)
  * **StreamingRunner** via SSE 🆕
* Custom

Your test harness already includes the web runner: `runner.bx or runner.cfm`.  You can execute that directly in your browser to get the results or run it via the CLI: `testbox run`.  We invite you to explore the different runners available to you.

{% content-ref url="commandbox-runner.md" %}
[commandbox-runner.md](commandbox-runner.md)
{% endcontent-ref %}

{% content-ref url="test-runner.md" %}
[test-runner.md](test-runner.md)
{% endcontent-ref %}

{% content-ref url="bundle-s-runner.md" %}
[bundle-s-runner.md](bundle-s-runner.md)
{% endcontent-ref %}

{% content-ref url="directory-runner.md" %}
[directory-runner.md](directory-runner.md)
{% endcontent-ref %}

{% content-ref url="ant-runner.md" %}
[ant-runner.md](ant-runner.md)
{% endcontent-ref %}

{% content-ref url="nodejs-runner.md" %}
[nodejs-runner.md](nodejs-runner.md)
{% endcontent-ref %}

{% content-ref url="global-runner.md" %}
[global-runner.md](global-runner.md)
{% endcontent-ref %}

{% content-ref url="test-browser.md" %}
[test-browser.md](test-browser.md)
{% endcontent-ref %}

{% content-ref url="testbox-run-ide.md" %}
[testbox-run-ide.md](testbox-run-ide.md)
{% endcontent-ref %}

{% content-ref url="streaming-runner.md" %}
[streaming-runner.md](streaming-runner.md)
{% endcontent-ref %}

### Custom Runners

However, you can create your own custom runners as long as you instantiate the `TestBox` class and execute one of it's runnable methods.  The main execution methods are:

```javascript
// Run tests and produce reporter results
testbox.run()

// Run tests and get raw testbox.system.TestResults object
testbox.runRaw()

// Run tests and produce reporter results from SOAP, REST, HTTP
testbox.runRemote()

// Run via Spec URL
http://localhost/tests/spec.cfc?method=runRemote

// Via CommandBox
testbox run
```

{% hint style="info" %}
We encourage you to read the [API docs](http://apidocs.ortussolutions.com/testbox/current) included in the distribution for the complete parameters for each method.
{% endhint %}

## `run()`&#x20;

Here are the arguments you can use for initializing TestBox or executing the `run()` method

```cfscript
/**
 * Run me some testing goodness, this can use the constructed object variables or the ones
 * you can send right here.
 *
 * @bundles      The path, list of paths or array of paths of the spec bundle classes to run and test
 * @directory    The directory to test which can be a simple mapping path or a struct with the following options: [ mapping = the path to the directory using dot notation (myapp.testing.specs), recurse = boolean, filter = closure that receives the path of the class found, it must return true to process or false to continue process ]
 * @reporter     The type of reporter to use for the results, by default is uses our 'simple' report. You can pass in a core reporter string type or an instance of a testbox.system.reports.IReporter. You can also pass a struct if the reporter requires options: {type="", options={}}
 * @labels       The list or array of labels that a suite or spec must have in order to execute.
 * @excludes     The list or array of labels that a suite or spec must not have in order to execute.
 * @options      A structure of configuration options that are optionally used to configure a runner.
 * @testBundles  A list or array of bundle names that are the ones that will be executed ONLY!
 * @testSuites   A list or array of suite names that are the ones that will be executed ONLY!
 * @testSpecs    A list or array of test names that are the ones that will be executed ONLY!
 * @callbacks    A struct of listener callbacks or a class with callbacks for listening to progress of the testing: onBundleStart,onBundleEnd,onSuiteStart,onSuiteEnd,onSpecStart,onSpecEnd
 * @eagerFailure If this boolean is set to true, then execution of more bundle tests will stop once the first failure/error is detected. By default this is false.
 */
any function run(
	any bundles,
	any directory,
	any reporter,
	any labels,
	any excludes,
	struct options,
	any testBundles      = [],
	any testSuites       = [],
	any testSpecs        = [],
	any callbacks        = {},
	boolean eagerFailure = false
)
```

* The `bundles` argument which can be a single CFC path or an array of CFC paths or a directory argument so it can go and discover the test bundles from that directory.&#x20;
* The `reporter` argument can be a core reporter name like: json,xml,junit,raw,simple,dots,tap,min,etc or it can be an instance of a reporter CFC.&#x20;
* You can execute the runners from any cfm template or any CFC or any URL, that is up to you.
