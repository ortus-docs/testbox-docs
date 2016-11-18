# Running Tests

TestBox ships with several test runners internally but we have tried to simplify and abstract it with our `testbox` object which can be found in the `testbox.system` package. The TestBox object allows you to execute tests from a variety of entry points and languages such as CFC, CFM, HTTP, NodeJS, SOAP or REST. You can also make your CFC's extend from our `BaseSpec` class so you can execute it directly via the URL. The main execution methods are:

```javascript
// Create TestBox object
testbox = new testbox.system.TestBox();

// You can add fluent specs via addDirectory(), addDirectories(), addBundles()
testbox.addDirectory( "/tests/specs" );

// Run tests and produce reporter results
testbox.run()

// Run tests and get raw testbox.system.TestResults object
testbox.runRaw()

// Run tests and produce reporter results from SOAP, REST, HTTP
testbox.runRemote()

// Run via Spec URL
http://localhost/test/spec.cfc?method=runRemote
```

> **Info** We encourage you to read the API docs included in the distribution for the complete parameters for each method.

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

## `runRemote()` arguments
Here are the arguments you can use for executing the `runRemote()` method:

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

The bundles argument which can be a single CFC path or an array of CFC paths or a directory argument so it can go and discover the test bundles from that directory. The reporter argument can be a core reporter name like: json,xml,junit,raw,simple,dots,tap,min,etc or it can be an instance of a reporter CFC. You can execute the runners from any cfm template or any CFC or any URL, that is up to you.

##Global Runner

TestBox ships with a global runner that can be used to run pretty much anything. You can customize it or place it wherever you need it:

<img src="../images/testbox-global-runner.png">

##Test Browser

TestBox ships with a test browser that is highly configurable to whatever URL accessible path you want. It will then show you a test browser where you can navigate and execute not only individual tests, but also directory suites as well.

<img src="../images/testbox-browser.png">

##ANT Runner

In our test samples and templates we include an ANT runner that will be able to execute your tests via ANT. It can also leverage our ANTJunit reporter to use the junitreport task to produce JUnit compliant reports as well. You can find this runner in the test samples and runner template directory.

##Bundle(s) Runner

```javascript
<cfset r = new coldbox.system.TestBox( "coldbox.testing.cases.testing.specs.BDDTest" ) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox( bundles="coldbox.testing.cases.testing.specs.BDDTest", testSpecs="OnlyThis,AndThis,AndThis" ) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox( bundles="coldbox.testing.cases.testing.specs.BDDTest", testSuites="Custom Matchers,A Spec" ) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox( [ "coldbox.testing.cases.testing.specs.BDDTest", "coldbox.testing.cases.testing.specs.BDD2Test" ] ) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox( bundles="coldbox.testing.cases.testing.specs.BDDTest", labels="railo" ) >
<cfoutput>#r.run(reporter="json")#</cfoutput>
```

<h3 style="color:grey">Test Runner</h3>
If you make your test bundle CFC inherit from our testbox.system.BaseSpec class, you will be able to execute the CFC directly via the URL:

```javascript
http://localhost/test/MyTest.cfc?method=runRemote
```

You can also pass the following arguments to the method via the URL:
* **testSuites** : A list or array of suite names that are the ones that will be executed ONLY!
* **testSpecs** : A list or array of test names that are the ones that will be executed ONLY!
* **reporter** : The type of reporter to run the test with

```javascript
http://localhost/test/MyTest.cfc?method=runRemote&reporter=json
```

<h3 style="color:grey">Directory Runner</h3>

```javascript
<cfset r = new coldbox.system.TestBox( directory="coldbox.testing.cases.testing.specs" ) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox( directory={ mapping="coldbox.testing.cases.testing.specs", recurse=false } ) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox(
      directory={ mapping="coldbox.testing.cases.testing.specs",
      recurse=true,
      filter=function(path){
            return ( findNoCase( "test", arguments.path ) ? true : false );
      }}) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new coldbox.system.TestBox(
      directory={ mapping="coldbox.testing.cases.testing.specs",
      recurse=true,
      filter=function(path){
            return ( findNoCase( "test", arguments.path ) ? true : false );
      }}) >
<cfset fileWrite( 'testreports.json', r.run() )>
```

##SOAP Runner

You can run tests via SOAP by leveraging the runRemote() method. The WSDL URL will be

```javascript
http://localhost/testbox/system/TestBox.cfc?wsdl
```

<h3 style="color:grey">HTTP/REST Runner</h3>
You can run tests via HTTP/REST by leveraging the runRemote() endpoint. The URL will be
```javascript
http://localhost/testbox/system/TestBox.cfc
```

##NodeJS Runner

![](../images/testbox-node.png)
There is a user-contributed NodeJS Runner that looks fantastic and can be downloaded here: https://www.npmjs.com/package/testbox-runner

Just use node to install:

```bash
npm install -g testbox-runner
```


