# What's New With 2.6.0

TestBox 2.6.0 is a minor release with some great new functionality and tons of fixes. You can find the release notes here and the major updates for this release.

## Chained DSL Improvements

The methods `aroundEach(), beforeEach() and afterEach()` can now be chained as they return the Base Spec as well. So get funky with the DSL.

## Null Support

You can now pass `null` into an expectation and TestBox will gracefully represent it instead of blowing up!

## Method Mocking Performance

Thanks to [Pixilation](https://github.com/pixilation) method mocking performance has sky rocketed. He changed the filename generation to an MD5 hash of the code, and disabled removing the files. This allows TestBox to leverage ColdFusion template caching. This reduces the time that `$()` takes from ~30 ms to ~0 ms. It is a happy day for mocking!

## Eager Failures

We have added a new argument to all TestBox `run(), runRemote(), runRaw()` commands: `eagerFailure`, which defaults to `false`. If this is turned on, then TestBox will gracefully short-circuit out of testing further Test Bundles if it finds any failures or errors on previous ones. This is useful for large suites that you want to stop testing if a failure is discovered.

## Help for the Color Blind

This was another community driven contribution by [Ian Burns - iwburns](https://github.com/iwburns) to help those folks with [Color Blindness](https://en.wikipedia.org/wiki/Color_blindness). You can see the differences in color in the github pull request: [https://github.com/Ortus-Solutions/TestBox/pull/60](https://github.com/Ortus-Solutions/TestBox/pull/60)

**Before**

![](https://user-images.githubusercontent.com/6172641/33219650-16ef7b06-d109-11e7-922c-6e50e3527f14.png)

**After**

![](https://user-images.githubusercontent.com/6172641/33219657-2900e3ca-d109-11e7-926d-bf78e2ee7362.png)

## Expanding of `x` exclusion Methods

You can now prefix the following methods with the letter `x` to exclude them from execution:

* `story()`
* `given()`
* `when()`
* `then()`
* `feature()`

## Release Notes

### Bugs

* \[[TESTBOX-196](https://ortussolutions.atlassian.net/browse/TESTBOX-196)\] - Assertion argument order does not match testbox.system.assertion for the expected argument
* \[[TESTBOX-199](https://ortussolutions.atlassian.net/browse/TESTBOX-199)\] - MockBox method subs are leaking whitespace
* \[[TESTBOX-203](https://ortussolutions.atlassian.net/browse/TESTBOX-203)\] - Labels in an it\(\) are ignored
* \[[TESTBOX-215](https://ortussolutions.atlassian.net/browse/TESTBOX-215)\] - The recurse checkbox in test-runner doesn't work
* \[[TESTBOX-216](https://ortussolutions.atlassian.net/browse/TESTBOX-216)\] - TestBox doesn't handle an interface CFC in the specs folder and chokes

### New Features

* \[[TESTBOX-195](https://ortussolutions.atlassian.net/browse/TESTBOX-195)\] - Return this from around,beforeEach and afterEach to have a chained DSL thanks to Jose Chavez

### Improvements

* \[[TESTBOX-200](https://ortussolutions.atlassian.net/browse/TESTBOX-200)\] - Output null representation in output message
* \[[TESTBOX-201](https://ortussolutions.atlassian.net/browse/TESTBOX-201)\] - wrap bundle runner execution in try/catch with rethrow
* \[[TESTBOX-202](https://ortussolutions.atlassian.net/browse/TESTBOX-202)\] - Improved method mocking performance
* \[[TESTBOX-204](https://ortussolutions.atlassian.net/browse/TESTBOX-204)\] - Provide a way to mute debug buffer on marshalled results
* \[[TESTBOX-205](https://ortussolutions.atlassian.net/browse/TESTBOX-205)\] - Add eager failure argument to runner so if set and a bundle fails no more bundles are tested after
* \[[TESTBOX-208](https://ortussolutions.atlassian.net/browse/TESTBOX-208)\] - Use correct key casing in report output
* \[[TESTBOX-210](https://ortussolutions.atlassian.net/browse/TESTBOX-210)\] - switch pass/failed/error colors to help color blindness
* \[[TESTBOX-211](https://ortussolutions.atlassian.net/browse/TESTBOX-211)\] - Exclude empty test suites from ANT JUnit Reporting
* \[[TESTBOX-212](https://ortussolutions.atlassian.net/browse/TESTBOX-212)\] - Add x- methods to skip suites and specs to new BDD methods.
* \[[TESTBOX-214](https://ortussolutions.atlassian.net/browse/TESTBOX-214)\] - Don't re-mock already mocked objects. \#63

