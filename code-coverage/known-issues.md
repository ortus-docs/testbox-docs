# Known Behaviors

There are a few known issues with code coverage that are currently out of our control.  We apoligize for any inconvenience they may give you.

### Occasional blank page on Lucee when running tests after restart

On some sites we've experienced that after a fresh restart of Lucee Server, when the first page you hit is the test suite and code coverage is enabled, Lucee throws a low level compilation error \(which you can see in the logs\) and a white page is returned to the browser.  We haven't figured out the scenario in which occurs, but refreshing the page always seems to work the second time.  

If you run into this on an automated build environment, consider configuring your build to hit a page first, or run the tests again if no results are captured the first run.  

### Very large CF files won't capture code coverage

This has been reported in Lucee for very large files of CF code.  Lucee automatically breaks bytecode into additional methods when compiling to avoid the JVM limits of maximum method sizes in a class file.  However, when FusionReactor instruments the byte code \(adds additional lines of code in\), it can push some of the internal methods over the JVM limit.  There will be an error logged to your console and TestBox will have no coverage for the file, showing every line as not executed.  

The only workaround at this time is to reduce the size of your CF files so the bytecode isn't as close to the JVM limits.  Try moving chunks of code out to includes.

### Adobe incorrectly calculates coverage for files which did not run at all

Only executable code should be tracked, meaning a comment, whitespace, or HTML that does not run will not count against your total coverage percentage.  When using Adobe ColdFusion, if there are any CF files which did not run at all \(and therefore were not compiled\) they will count every line in the file as executable since FusionReactor is only capable of tracking files which are compiled.  Lucee has a workaround to manually force compilation on such files, but Adobe does not.

The best work around is to improve your tests so that these files are being executed!  Alternatively, you can add those files to the blacklist until you are ready to write tests for them, but that will make your coverage look better than it really is if you do eventually want to write tests for those files.  Minimally, a test that does nothing but create an instance of a CFC would be enough to trigger its compilation so correct coverage can kick in.

### Why didn't this ending curly brace, etc get tracked correctly?

Occasionally you may run across some lines of code in the Code Coverage Browser report that doesn't seem correctly tracked.  Common offenders are extra curly braces or ending tags floating on a line of their own.  The fact is, mapping each line of CF code to the actual lines of compiled bytecode is tricky business and done entirely by the CF engines \(Adobe, Lucee\) under the hood during compilation.  Sometimes bits of code might not seem tracked correctly, but we've never seen it have any significant effect on your overall coverage data.  The behavior is specific to each engine/version but typically lines like that just get associated with the next executable line, so once your coverage for a file hits all the possible lines, the issue goes away :\)  Feel free to report any issues you see.  We have some workarounds in place and can see about making it better.

### My coverage statistics all of a sudden show 0% or weird numbers for code I know that is runnning

TestBox's code coverage feature relies on byte code instrumentation from Fusion Reactor. It seems that in some instances this process can fall over due to FR's internal behaviour, byte code class caching, internal compilation changes going from one version of your CFML engine to another one and other reasons. While we report these kind of issues upstream, unfortunately they are nearly impossible for us to properly investigate and debug.

One common symptom seems to be that code coverage statistics for individual files or your overall codebase vary extremely between two TestBox executions without having changed code at all or in a meaningful way. Another symptom observed by users is that code coverage drops to 0% for certain files and you know for sure that these files would be executed during your tests.

A restart of your CFML engine and a subsequent run of your tests with code coverage usually fixes this problem.
