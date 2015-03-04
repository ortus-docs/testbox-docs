# Bundle(s) Runner

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
