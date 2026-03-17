---
icon: code-simple
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/running-tests/bundle-s-runner
---

# Bundle(s) Runner

This is more of an approach than an actual specifc runner.  This approach shows you that you can create a script file in BoxLang (`bxs`) or in CFML (`cfs|cfm`) that can in turn execute any test bundle(s) with many many runnable configurations.

### BoxLang

The BoxLang language allows you to run your scripts via the CLI or the browser if you have a web server attached to your project.

{% code title="run.bxs" %}
```cfscript
// Test the BDD Bundle
r = new testbox.system.TestBox( "tests.specs.BDDTest" )
println( r.run() );

// Test the bundle with ONLY the passed specs
r = new testbox.system.TestBox( "tests.specs.BDDTest" )
println( r.run( testSpecs="OnlyThis,AndThis,AndThis" ) )

// Test the bundle with ONLY the passed suites
r = new testbox.system.TestBox( "tests.specs.BDDTest" )
println( r.run( testSuites="Custom Matchers,A Spec" ) )

// Test the passed array of bundles
r = new testbox.system.TestBox( [ "tests.specs.BDDTest", "tests.specs.BDD2Test" ] )
println( r.run() )

// Test with labels and the minimal reporter
r = new testbox.system.TestBox( bundles: "tests.specs.BDDTest", labels="linux" )
println( r.run( reporter: "mintext" ) )
```
{% endcode %}

If you want to run it in the CLI, then just use:

```bash
boxlang run.bxs
```

If you want to run it via the web server, place it in your `/tests/` folder and run it

```
http://localhost/tests/run.bxs
```

### CFML

CFML engines only allow you to run tests via the browser.  So create your script, place it in your web accessible `/tests` folder and run it.

{% code title="run.cfm" %}
```cfscript
<cfscript>
	// Test the BDD Bundle
	r = new testbox.system.TestBox( "tests.specs.BDDTest" )
	writeOutput( r.run() );
	
	// Test the bundle with ONLY the passed specs
	r = new testbox.system.TestBox( "tests.specs.BDDTest" )
	writeOutput( r.run( testSpecs="OnlyThis,AndThis,AndThis" ) )
	
	// Test the bundle with ONLY the passed suites
	r = new testbox.system.TestBox( "tests.specs.BDDTest" )
	writeOutput( r.run( testSuites="Custom Matchers,A Spec" ) )
	
	// Test the passed array of bundles
	r = new testbox.system.TestBox( [ "tests.specs.BDDTest", "tests.specs.BDD2Test" ] )
	writeOutput( r.run() )
	
	// Test with labels and the minimal reporter
	r = new testbox.system.TestBox( bundles: "tests.specs.BDDTest", labels="linux" )
	writeOutput( r.run( reporter: "mintext" ) )
</cfscript>
```
{% endcode %}
