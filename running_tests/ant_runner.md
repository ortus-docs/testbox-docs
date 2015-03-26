# ANT Runner

In our test samples and templates we include an ANT runner that will be able to execute your tests via ANT. It can also leverage our ANTJunit reporter to use the junitreport task to produce JUnit compliant reports as well. You can find this runner in the test samples and runner template directory.

```javascript
<cfsetting showDebugOutput="false">
<cfparam name="url.reporter"       default="simple">
<cfparam name="url.directory"      default="test.specs">
<cfparam name="url.recurse"        default="true" type="boolean">
<cfparam name="url.bundles"        default="">
<cfparam name="url.labels"         default="">
<cfparam name="url.reportpath"     default="#expandPath( "/tests/results" )#">
<cfscript>
// prepare for tests for bundles or directories
if( len( url.bundles ) ){
     testbox = new testbox.system.TestBox( bundles=url.bundles, labels=url.labels );
}
else{
     testbox = new testbox.system.TestBox( directory={ mapping=url.directory, recurse=url.recurse}, labels=url.labels );
}
// Run Tests using correct reporter
results = testbox.run( reporter=url.reporter );
// do stupid JUnitReport task processing, if the report is ANTJunit
if( url.reporter eq "ANTJunit" ){
     xmlReport = xmlParse( results );
     for( thisSuite in xmlReport.testsuites.XMLChildren ){
          fileWrite( url.reportpath & "/TEST-" & thisSuite.XMLAttributes.name & ".xml", toString( thisSuite ) );
     }
}
// Writeout Results
writeoutput( results );
</cfscript>
```
<h3 style="color:grey">Ant</h3>

This build is our global build for running TestBox via ANT.

```javascript
<?xml version="1.0"?>
<--<
This ANT build can be used to execute your tests with automation using our included runner.cfm.
You can executes directories, bundles and so much more.  It can also produce JUnit reports using the
ANT junitreport tag.  This is meant to be a template for you to spice up.

There are two targets you can use: run and run-junit

Execute the default 'run' target
ant -f test.xml
OR
ant -f test.xml run

Execute the 'run-junit' target
ant -f test.xml run-junit

PLEASE NOTE THAT YOU MUST ALTER THE RUNNER'S URL ACCORDING TO YOUR ENVIRONMENT.
-->
<project name="testbox-ant-runner" default="run" basedir=".">

     <-- <THE URL TO THE RUNNER, PLEASE CHANGE ACCORDINGLY-->
     <property name="url.runner"             value="http://cf10cboxdev.jfetmac/coldbox/ApplicationTemplates/Advanced/tests/runner.cfm?"/>
     <-- <FILL OUT THE BUNDLES TO TEST, CAN BE A LIST OF CFC PATHS -->
     <property name="test.bundles"      value="" />
     <-- <FILL OUT THE DIRECTORY MAPPING TO TEST -->
     <property name="test.directory"         value="test.specs" />
     <-- <FILL OUT IF YOU WANT THE DIRECTORY RUNNER TO RECURSE OR NOT -->
     <property name="test.recurse"      value="true" />
     <-- <FILL OUT THE LABELS YOU WANT TO APPLY TO THE TESTS -->
     <property name="test.labels"       value="" />
     <-- <FILL OUT THE TEST REPORTER YOU WANT, AVAILABLE REPORTERS ARE: ANTJunit, Codexwiki, console, dot, doc, json, junit, min, raw, simple, tap, text, xml -->
     <property name="test.reporter"          value="simple" />
     <-- <FILL OUT WHERE REPORTING RESULTS ARE STORED -->
     <property name="report.dir"        value="${basedir}/results" />
     <property name="junitreport.dir"   value="${report.dir}/junitreport" />

     <target name="init" description="Init the tests">
          <mkdir dir="${junitreport.dir}" />
          <tstamp prefix="start">
               <format property="TODAY" pattern="MM-dd-YYYY hh:mm:ss aa"/>
          </tstamp>
          <concat destfile="${report.dir}/latestrun.log">Tests ran at ${start.TODAY}</concat>
     </target>

     <target name="run" depends="init" description="Run our tests and produce awesome results">

          <-- <Directory Runner
               Executes recursively all tests in the passed directory and stores the results in the
               'dest' param.  So if you want to rename the file, do so here.

                Available params for directory runner:
                - Reporter
                - Directory
                - Recurse
                - Labels
          -->
          <get dest="${report.dir}/results.html"
                src="${url.runner}&directory=${test.directory}&recurse=${test.recurse}&reporter=${test.reporter}&labels=${test.labels}"
                verbose="true"/>

          <-- <Bundles Runner
               You can also run tests for specific bundles by using the runner with the bundles params

               Available params for runner:
                - Reporter
                - Bundles
                - Labels

          <get dest="${report.dir}/results.html"
                src="${url.runner}&bundles=${test.bundles}&reporter=${test.reporter}&labels=${test.labels}"
                verbose="true"/>
           -->

     </target>

     <target name="run-junit" depends="init" description="Run our tests and produce ANT JUnit reports">

          <-- <Directory Runner
               Executes recursively all tests in the passed directory and stores the results in the
               'dest' param.  So if you want to rename the file, do so here.

                Available params for directory runner:
                - Reporter = ANTJunit fixed
                - Directory
                - Recurse
                - Labels
                - ReportPath : The path where reports will be stored, by default it is the ${report.dir} directory.
          -->
          <get dest="${report.dir}/results.xml"
                src="${url.runner}&directory=${test.directory}&recurse=${test.recurse}&reporter=ANTJunit&labels=${test.labels}&reportPath=${report.dir}"
                verbose="true"/>

          <-- <Bundles Runner
               You can also run tests for specific bundles by using the runner with the bundles params

               Available params for runner:
                - Reporter
                - Bundles
                - Labels

          <get dest="${report.dir}/results.html"
                src="${url.runner}&bundles=${test.bundles}&reporter=${test.reporter}&labels=${test.labels}"
                verbose="true"/>
           -->

           <-- <Create fancy junit reports -->
          <junitreport todir="${junitreport.dir}">
               <fileset dir="${report.dir}">
                    <include name="TEST-*.xml"/>
               </fileset>
               <report format="frames" todir="${junitreport.dir}">
                    <param name="TITLE" expression="My Awesome TestBox Results"/>
               </report>
          </junitreport>

     </target>

</project>
```

