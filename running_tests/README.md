# Running Tests

TestBox ships with several test runners internally but we have tried to simplify and abstract it with our TestBox object which can be found in the `testbox.system package`. The `testbox` object allows you to execute tests from a CFC, CFM, HTTP, SOAP, NodeJS or REST. You can also make your CFC's extend from our `BaseSpec` class so you can execute it directly via the URL. The main execution methods are:

```javascript
// Run tests and produce reporter results
testbox.run()

// Run tests and get raw testbox.system.TestResults object
testbox.runRaw()

// Run tests and produce reporter results from SOAP, REST, HTTP
testbox.runRemote()

// Run via Spec URL
http://localhost/tests/spec.cfc?method=runRemote
```

> We encourage you to read the [API docs](http://apidocs.ortussolutions.com/testbox/current) included in the distribution for the complete parameters for each method.

## `run()` Arguments

Here are the arguments you can use for initializing TestBox or executing the `run()` method

|Argument|Required|Default|Type|Description|
|--|--|--|--|--|
|bundles|true|---|The path, list of paths or array of paths of the spec bundle CFCs to run and test|
|directory |false |---|struct |The directory mapping path or a struct: [ mapping = the path to the directory using dot notation (myapp.testing.specs), recurse = boolean, filter = closure that receives the path of the CFC found, it must return true to process or false to continue process ]|
|reporter |false|simple|string/struct/instance|The type of reporter to use for the results, by default is uses our 'simple' report. You can pass in a core reporter string type or an instance of a coldbox.system.reports.IReporter. You can also pass a struct with [type="string or classpath", options={}] if a reporter expects options.|
|labels|false|false|string/array|The string or array of labels the runner will use to execute suites and specs with.|
|options|false|{}|struct|A structure of property name-value pairs that each runner can implement and use at its discretion.|
|testBundles |false|---|string/array|A list or array of bundle names that are the ones that will be executed ONLY!|
|testSuites |false |---|string/array |A list or array of suite names that are the ones that will be executed ONLY! |
|testSpecs |false|---|string/array|A list or array of test names that are the ones that will be executed ONLY|

## `runRemote()` Arguments
Here are the arguments you can use for executing the `runRemote()` method of the TestBox object:

|Argument|Required|Default|Type|Description|
|--|--|--|--|--|
|bundles |true|---|string|The path, list of paths or array of paths of the spec bundle CFCs to run and test|
|directory|false|---|string|The directory mapping to test: directory = the path to the directory using dot notation (myapp.testing.specs)|
|recurse|false|true|boolean|Recurse the directory mapping or not, by default it does|
|reporter|false|simple|string/path|The type of reporter to use for the results, by default is uses our 'simple' report. You can pass in a core reporter string type or a class path to the reporter to use.|
|reporterOptions |false|{}|JSON|A JSON struct literal of options to pass into the reporter|
|labels |false|false|string|The string array of labels the runner will use to execute suites and specs with.|
|options |false|{}|JSON|A JSON struct literal of configuration options that are optionally used to configure a runner.|
|testBundles |false|---|string/array|A list or array of bundle names that are the ones that will be executed ONLY!|
|testSuites|false|---|string|A list of suite names that are the ones that will be executed ONLY! |
|testSpecs |false|---|string|A list of test names that are the ones that will be executed ONLY|

* The `bundles` argument which can be a single CFC path or an array of CFC paths or a directory argument so it can go and discover the test bundles from that directory. 
* The `reporter` argument can be a core reporter name like: json,xml,junit,raw,simple,dots,tap,min,etc or it can be an instance of a reporter CFC. 
* You can execute the runners from any cfm template or any CFC or any URL, that is up to you.















