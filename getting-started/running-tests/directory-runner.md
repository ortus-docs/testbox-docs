---
icon: folder-tree
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/running-tests/directory-runner
---

# Directory Runner

This is more of an approach than an actual specifc runner.  This approach shows you that you can create a script file in BoxLang (`bxs`) or in CFML (`cfs|cfm`) that can in turn execute any test directory with many many runnable configurations.  It is very similar to the [Bundle Runner](bundle-s-runner.md) approach.

### BoxLang

The BoxLang language allows you to run your scripts via the CLI or the browser if you have a web server attached to your project.

{% code title="run.bxs" %}
```groovy
// Run all the specs in the tests.specs directory and subdirectories
r = new testbox.system.TestBox( directory="tests.specs" )
println( r.run() )

// Run all the specs in the tests.specs directory ONLY
r = new testbox.system.TestBox(
		directory={
            mapping="tets.specs",
            recurse=false
      } )
println( r.run() )

// Run all the specs in the tests.specs directory and subdirectories using
// a custom lambda filter
r = new testbox.system.TestBox(
      	directory={
            mapping : "tets.specs",
            filter : path -> findNoCase( "test", arguments.path ) ? true : false
      }) >
println( r.run() )

// Run all the specs in the tests.specs directory and subdirectories using
// a custom lambda filter and create a JSON report
r = new testbox.system.TestBox(
      	directory={
            mapping="tets.specs",
            filter : path -> findNoCase( "test", arguments.path ) ? true : false
      }) >
fileWrite( 'testreports.json', r.run() )
println( "JSON report created" )
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

```xml
<cfset r = new testbox.system.TestBox( directory="tests.specs" ) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new testbox.system.TestBox(
      directory={
            mapping="tets.specs",
            recurse=false
      }) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new testbox.system.TestBox(
      directory={
            mapping="tets.specs",
            recurse=true,
            filter=function(path){
                  return ( findNoCase( "test", arguments.path ) ? true : false );
            }
      }) >
<cfoutput>#r.run()#</cfoutput>

<cfset r = new testbox.system.TestBox(
      directory={
            mapping="tets.specs",
            recurse=true,
            filter=function(path){
                  return ( findNoCase( "test", arguments.path ) ? true : false );
            }
      }) >
<cfset fileWrite( 'testreports.json', r.run() )>
```

