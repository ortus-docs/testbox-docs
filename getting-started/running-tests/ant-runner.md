---
icon: ant
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/running-tests/ant-runner
---

# ANT Runner

In our test harness we include an ANT runner (`test.xml`) that will be able to execute your tests via ANT.&#x20;

{% hint style="info" %}
**Ant** is a Java-based build tool from Apache designed to automate the software build process. Unlike traditional build tools that rely on shell commands, Ant uses XML to describe the build process and its dependencies, making it platform-independent and flexible. It is particularly useful for Java projects to compile code, manage dependencies, and create deployment packages.\
[https://ant.apache.org/bindownload.cgi](https://ant.apache.org/bindownload.cgi)
{% endhint %}

It can also leverage our `ANTJunit` reporter to use the `junitreport` task to produce JUnit compliant reports as well.

```xml
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
     <property name="url.runner"             value="http://localhost/tests/runner.cfm?"/>
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

Now you can run the commands

```bash
# Execute the default 'run' target
ant -f test.xml
# Execute a specific target
ant -f test.xml run-junit
```
