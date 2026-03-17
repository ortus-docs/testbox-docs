---
icon: globe-wifi
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/running-tests/test-runner
---

# Web Runner

Every test harness comes with a `runner.bx or runner.cfm` in the root of the `tests` folder.  This is called the web runner and is executable via the web server you are running your application on.  This will execute all the tests by convention found in the `tests/specs` folder.

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

```
http://localhost/tests/runner.cfm
```

You can open that file and customize it as you see fit.  Here is an example of such a file:

```xml
<!--- Executes all tests in the 'specs' folder with simple reporter by default --->
<bx:param name="url.reporter" 			default="simple">
<bx:param name="url.directory" 			default="tests.specs">
<bx:param name="url.recurse" 			default="true" type="boolean">
<bx:param name="url.bundles" 			default="">
<bx:param name="url.labels" 			 default="">
<bx:param name="url.excludes" 			 default="">
<bx:param name="url.reportpath" 		 default="#expandPath( "/tests/results" )#">
<bx:param name="url.propertiesFilename"  default="TEST.properties">
<bx:param name="url.propertiesSummary"	default="false" type="boolean">
<bx:param name="url.editor" 			  default="vscode">
<bx:param name="url.bundlesPattern" 	 default="*Spec*.cfc|*Test*.cfc|*Spec*.bx|*Test*.bx">

<!--- Code Coverage requires FusionReactor --->
<bx:param name="url.coverageEnabled"			default="false">
<bx:param name="url.coveragePathToCapture"		default="#expandPath( '/root' )#">
<bx:param name="url.coverageWhitelist"			  default="">
<bx:param name="url.coverageBlacklist"			  default="/testbox,/coldbox,/tests,/modules,Application.cfc,/index.cfm,Application.bx,/index.bxm">
<!---<bx:param name="url.coverageBrowserOutputDir"		default="#expandPath( '/tests/results/coverageReport' )#">--->
<!---<bx:param name="url.coverageSonarQubeXMLOutputPath"	default="#expandPath( '/tests/results/SonarQubeCoverage.xml' )#">--->
<!--- Enable batched code coverage reporter, useful for large test bundles which require spreading over multiple testbox run commands. --->
<!--- <bx:param name="url.isBatched"						default="false"> --->

<!--- Include the TestBox HTML Runner --->
<bx:include template="/testbox/system/runners/HTMLRunner.cfm" >
```

### Test Bundle Execution

If you make your test bundle class inherit from our `testbox.system.BaseSpec` class, you will be able to execute the class directly via the URL:

```javascript
// BoxLang
http://localhost/tests/specs/MyFirstTest.bx?method=runRemote

// CFML
http://localhost/tests/specs/MyFirstTest.cfc?method=runRemote
```

### Arguments

All the arguments found in the `runner` are available as well in a direct bundle execution:

* `labels`: The labels to apply to the execution
* `testMethod` : A list or array of xunit test names that will be executed ONLY!
* `testSuites` : A list or array of suite names that are the ones that will be executed ONLY!
* `testSpecs` : A list or array of test names that are the ones that will be executed ONLY!
* `reporter` : The type of reporter to run the test with

```javascript
// BoxLang
http://localhost/tests/specs/MyFirstTest.bx?method=runRemote&reporter=text

// CFML
http://localhost/tests/specs/MyFirstTest.cfc?method=runRemote&reporter=text
```
