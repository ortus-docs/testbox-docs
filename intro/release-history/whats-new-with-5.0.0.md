---
description: May 10, 2022
---

# What's New With 5.0.0

#### Fixed

* [TESTBOX-341](https://ortussolutions.atlassian.net/browse/TESTBOX-341) toHaveLength param should be numeric
* [TESTBOX-354](https://ortussolutions.atlassian.net/browse/TESTBOX-354) Element $DEBUGBUFFER is undefined in THIS
* [TESTBOX-356](https://ortussolutions.atlassian.net/browse/TESTBOX-356) Don't assume TagContext has length on simple reporter
* [TESTBOX-357](https://ortussolutions.atlassian.net/browse/TESTBOX-357) notToThrow() incorrectly passes when no regex is specified
* [TESTBOX-360](https://ortussolutions.atlassian.net/browse/TESTBOX-360) full null support not working on Application env test
* [TESTBOX-361](https://ortussolutions.atlassian.net/browse/TESTBOX-361) MockBox Suite: Key \[aNull] doesn't exist
* [TESTBOX-362](https://ortussolutions.atlassian.net/browse/TESTBOX-362) Cannot create subfolders within testing spec directories.

#### Improvements

* [TESTBOX-333](https://ortussolutions.atlassian.net/browse/TESTBOX-333) Add contributing.md to the repo
* [TESTBOX-339](https://ortussolutions.atlassian.net/browse/TESTBOX-339) full null support automated testing
* [TESTBOX-353](https://ortussolutions.atlassian.net/browse/TESTBOX-353) allows globbing path patterns in test bundles argument
* [TESTBOX-355](https://ortussolutions.atlassian.net/browse/TESTBOX-355) Add debugBuffer to JSONReporter
* [TESTBOX-366](https://ortussolutions.atlassian.net/browse/TESTBOX-366) ANTJunit Reporter better visualization of the failed origin and details
* [TESTBOX-368](https://ortussolutions.atlassian.net/browse/TESTBOX-368) Support list of Directories for HTMLRunner to allow a more modular tests structure
* [TESTBOX-370](https://ortussolutions.atlassian.net/browse/TESTBOX-370) \`toHaveKey\` works on queries in Lucee but not ColdFusion

#### Added

* [TESTBOX-371](https://ortussolutions.atlassian.net/browse/TESTBOX-371) Add CoverageReporter for batching code coverage reports
* [TESTBOX-137](https://ortussolutions.atlassian.net/browse/TESTBOX-137) Ability to spy on existing methods: $spy()
* [TESTBOX-342](https://ortussolutions.atlassian.net/browse/TESTBOX-342) Add development dependencies to box.json
* [TESTBOX-344](https://ortussolutions.atlassian.net/browse/TESTBOX-344) Performance optimizations for BaseSpec creations by lazy loading external objects
* [TESTBOX-345](https://ortussolutions.atlassian.net/browse/TESTBOX-345) add a skip(\[message]) like fail() for skipping from inside a spec
* [TESTBOX-365](https://ortussolutions.atlassian.net/browse/TESTBOX-365) New build process using CommandBox
* [TESTBOX-372](https://ortussolutions.atlassian.net/browse/TESTBOX-372) Adobe 2023 and Lucee 6 Support
