---
description: May 10, 2022
---

# What's New With 5.0.0

TestBox 5.x series is a major bump in our library.  Here are the major areas of improvement and the full release notes.

## Engine Support

<figure><img src="../../.gitbook/assets/testbox-engine-support.png" alt=""><figcaption><p>TestBox Engine Support</p></figcaption></figure>

We have dropped Adobe 2016 support and added support for Adobe 2023 and Lucee 6+

## Batch Test Coverage Reporting

<figure><img src="../../.gitbook/assets/TestBox Batch CodeCoverage.png" alt="" width="250"><figcaption><p>TestBox Batch Code Coverage</p></figcaption></figure>

Due to memory limitations in CI environments, larger codebases cannot run all tests as a single `testbox run` command. Instead, specs are run in a methodical folder-by-folder sequence, separating the `testbox run` out over many requests and thus working around the Out-Of-Memory exceptions.

While this works, it prevents accurate code coverage reporting since only a small portion of the tests are executed during any request. The generated code coverage report only shows a tiny fraction of the coverage - say, 2% - and not the whole picture

TestBox 5 introduces a `CoverageReporter` component which

1. Runs on every TestBox code coverage execution
2. Loads any previous coverage data from a JSON file
3. Combines the previous coverage data with the current execution's coverage data (file by file and line by line)
4. Persists the COMBINED coverage data to a JSON file.
5. Returns the COMBINED coverage data for the `CoverageBrowser.cfc` to build as an HTML report

When setting `url.isBatched=true` and executing the batched test runner, the code coverage report will grow with each sequential `testbox run` command.

## Method Spies!

<figure><img src="../../.gitbook/assets/TestBox Method Spies.png" alt="" width="375"><figcaption></figcaption></figure>

MockBox now supports a `$spy( method )` method that allows you to spy on methods with all the call log goodness but without removing all the methods.  Every other method remains intact, and the actual spied method remains active.  We decorate it to track its calls and return data via the `$callLog()` method.

**Example of CUT:**

```cfscript
void function doSomething(foo){
  // some code here then...
  local.foo = variables.collaborator.callMe(local.foo);
  variables.collaborator.whatever(local.foo);
}
```

**Example Test:**

```cfscript
function test_it(){
  local.mocked = createMock( "com.foo. collaborator" );
  variables.CUT.$property( "collaborator", "variables", local.mocked );
  assertEquals( 1, local.mocked.$count( "callMe" ) );
  assertEquals( 1, local.mocked.$count( "whatever" ) );
}
```

## Performance Improvements

<figure><img src="../../.gitbook/assets/TestBox Performance Icon.png" alt="" width="250"><figcaption></figcaption></figure>

We have focused on this release to lazy load everything as much as possible to allow for much better testing performance.  Check it out!

## Skip it! Skip it -> Good!

<figure><img src="../../.gitbook/assets/TestBox Skip Feature.png" alt="" width="250"><figcaption></figcaption></figure>

You can now use the `skip( message )` method to skip any spec or suite a-la-carte instead of as an argument to the function definitions.  This lets you programmatically skip certain specs and suites and pass a nice message.

```cfscript
it( "can do something", () => {
    ...
    if( condition ){
        skip( "Condition is true, skipping spec" )
    }
    ...
} )
```

## Release Notes

### Fixed

* [TESTBOX-341](https://ortussolutions.atlassian.net/browse/TESTBOX-341) `toHaveLength` param should be numeric
* [TESTBOX-354](https://ortussolutions.atlassian.net/browse/TESTBOX-354) Element `$DEBUGBUFFER` is undefined in `THIS`
* [TESTBOX-356](https://ortussolutions.atlassian.net/browse/TESTBOX-356) Don't assume `TagContext` has length on simple reporter
* [TESTBOX-357](https://ortussolutions.atlassian.net/browse/TESTBOX-357) `notToThrow()` incorrectly passes when no regex is specified
* [TESTBOX-360](https://ortussolutions.atlassian.net/browse/TESTBOX-360) full null support not working on Application env test
* [TESTBOX-361](https://ortussolutions.atlassian.net/browse/TESTBOX-361) MockBox Suite: Key \[aNull] doesn't exist
* [TESTBOX-362](https://ortussolutions.atlassian.net/browse/TESTBOX-362) Cannot create subfolders within testing spec directories.

### Improvements

* [TESTBOX-333](https://ortussolutions.atlassian.net/browse/TESTBOX-333) Add `contributing.md` to the repo
* [TESTBOX-339](https://ortussolutions.atlassian.net/browse/TESTBOX-339) full null support automated testing
* [TESTBOX-353](https://ortussolutions.atlassian.net/browse/TESTBOX-353) allows globbing path patterns in test bundles argument
* [TESTBOX-355](https://ortussolutions.atlassian.net/browse/TESTBOX-355) Add `debugBuffer` to `JSONReporter`
* [TESTBOX-366](https://ortussolutions.atlassian.net/browse/TESTBOX-366) ANTJunit Reporter better visualization of the failed origin and details
* [TESTBOX-368](https://ortussolutions.atlassian.net/browse/TESTBOX-368) Support list of Directories for HTMLRunner to allow a more modular tests structure
* [TESTBOX-370](https://ortussolutions.atlassian.net/browse/TESTBOX-370) \`toHaveKey\` works on queries in Lucee but not ColdFusion

### Added

* [TESTBOX-371](https://ortussolutions.atlassian.net/browse/TESTBOX-371) Add `CoverageReporter` for batching code coverage reports
* [TESTBOX-137](https://ortussolutions.atlassian.net/browse/TESTBOX-137) Ability to spy on existing methods: $spy()
* [TESTBOX-342](https://ortussolutions.atlassian.net/browse/TESTBOX-342) Add development dependencies to box.json
* [TESTBOX-344](https://ortussolutions.atlassian.net/browse/TESTBOX-344) Performance optimizations for BaseSpec creations by lazy loading external objects
* [TESTBOX-345](https://ortussolutions.atlassian.net/browse/TESTBOX-345) add a `skip([message])` like fail() for skipping from inside a spec
* [TESTBOX-365](https://ortussolutions.atlassian.net/browse/TESTBOX-365) New build process using CommandBox
* [TESTBOX-372](https://ortussolutions.atlassian.net/browse/TESTBOX-372) Adobe 2023 and Lucee 6 Support
