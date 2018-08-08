# What's New With 2.7.0

TestBox 2.7.0 is a minor release with some great new functionality and tons of fixes. You can find the release notes here and the major updates for this release.

## Release Notes

### Bugs

* \[[TESTBOX-217](https://ortussolutions.atlassian.net/browse/TESTBOX-217)\] - new version of Lucee has complex values in UDF implementation that breaks the mock generator
* \[[TESTBOX-222](https://ortussolutions.atlassian.net/browse/TESTBOX-222)\] - Heap issues and stack overflow issues when normalizing ORM entities in mocking arguments

### New Features

* \[[TESTBOX-221](https://ortussolutions.atlassian.net/browse/TESTBOX-221)\] - Complete refactoring of around,before,after each executions to support concurrency

### Improvements

* \[[TESTBOX-219](https://ortussolutions.atlassian.net/browse/TESTBOX-219)\] - some refactoring to use `invoke()` instead of `evaluate()`
* \[[TESTBOX-220](https://ortussolutions.atlassian.net/browse/TESTBOX-220)\] - some speed improvements by not using `createuuid` anymore

