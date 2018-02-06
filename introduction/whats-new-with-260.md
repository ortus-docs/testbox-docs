# What's New With 2.6.0

TestBox 2.6.0 is a minor release with some great new functionality and tons of fixes.  You can find the release notes here and the major updates for this release.

## Chained DSL Improvements

The methods `aroundEach(), beforeEach() and afterEach()` can now be chained as they return the Base Spec as well.  So get funky with the DSL.

## Null Support

You can now pass `null` into an expectation and TestBox will gracefully represent it instead of blowing up!

## Method Mocking Performance

Thanks to [Pixilation](https://github.com/pixilation) method mocking performance has sky rocketed.  He changed the filename generation to an MD5 hash of the code, and disabled removing the files. This allows TestBox to leverage ColdFusion template caching. This reduces the time that `$()` takes from ~30 ms to ~0 ms.  It is a happy day for mocking!

## Eager Failures

We have added a new argument to all TestBox `run` commands: `eagerFailure`, which defaults to `false`.  If this is turned on, then TestBox will gracefully short-circuit out of testing further Test Bundles if it finds any failures or errors on previous ones.  This is useful for large suites that you want to stop testing if a failure is discovered.


## Release Notes       

### Bugs

* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-196'>TESTBOX-196</a>] - Assertion argument order does not match testbox.system.assertion for the expected argument
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-199'>TESTBOX-199</a>] - MockBox method subs are leaking whitespace
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-203'>TESTBOX-203</a>] - Labels in an it() are ignored
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-215'>TESTBOX-215</a>] - The recurse checkbox in test-runner doesn&#39;t work
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-216'>TESTBOX-216</a>] - TestBox doesn&#39;t handle an interface CFC in the specs folder and chokes

### New Features

* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-195'>TESTBOX-195</a>] - Return this from around,beforeEach and afterEach to have a chained DSL thanks to Jose Chavez
        
### Improvements

* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-200'>TESTBOX-200</a>] - Output null representation in output message
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-201'>TESTBOX-201</a>] - wrap bundle runner execution in try/catch with rethrow
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-202'>TESTBOX-202</a>] - Improved method mocking performance
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-204'>TESTBOX-204</a>] - Provide a way to mute debug buffer on marshalled results
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-205'>TESTBOX-205</a>] - Add eager failure argument to runner so if set and a bundle fails no more bundles are tested after
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-208'>TESTBOX-208</a>] - Use correct key casing in report output
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-210'>TESTBOX-210</a>] - switch pass/failed/error colors to help color blindness
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-211'>TESTBOX-211</a>] - Exclude empty test suites from ANT JUnit Reporting
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-212'>TESTBOX-212</a>] - Add x- methods to skip suites and specs to new BDD methods.
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-214'>TESTBOX-214</a>] - Don&#39;t re-mock already mocked objects. #63
             