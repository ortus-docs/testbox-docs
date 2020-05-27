# What's New With 4.1.0

TestBox v4.1.0 is a minor update. Quickly update it via CommandBox

```text
update testbox
```

### Bugs

* \[[TESTBOX-281](https://ortussolutions.atlassian.net/browse/TESTBOX-281)\] - request.testbox:  Component ... has no accessible Member with name \[$TESTID\]
* \[[TESTBOX-283](https://ortussolutions.atlassian.net/browse/TESTBOX-283)\] - Fix type on test results for bundlestats
* \[[TESTBOX-286](https://ortussolutions.atlassian.net/browse/TESTBOX-286)\] - DebugBuffer was being removed instead of resetting to empty for getMemento

### New Features

* \[[TESTBOX-282](https://ortussolutions.atlassian.net/browse/TESTBOX-282)\] - Added cfml engine and version as part of the test results as properties
* \[[TESTBOX-284](https://ortussolutions.atlassian.net/browse/TESTBOX-284)\] - Update all reporters so they can just build and return the report with no content type or context repsonse resets
* \[[TESTBOX-285](https://ortussolutions.atlassian.net/browse/TESTBOX-285)\] - make buildReporter public in the testbox core

