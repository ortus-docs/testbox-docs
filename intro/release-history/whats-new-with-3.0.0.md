# What's New With 4.0.0



TestBox 4.0.0 is a major release.  It has compatibility changes that you should be aware and lots of good features!  

### Compatibility

The major compatibility issues are the engine support removals:

* Lucee 4.5 Support Dropped
* Adobe ColdFusion 11 Dropped

### Updating

It is easy to update, just type `update testbox` and you are done!

## Major Features

### TestBox Output Utilities

### Mocking Data



## Release Notes

### Bugs

* \[[TESTBOX-275](https://ortussolutions.atlassian.net/browse/TESTBOX-275)\] - Exception in `beforeTests`/`afterTests` not reported in a meaningful way on the ANT Junit Reporter
* \[[TESTBOX-278](https://ortussolutions.atlassian.net/browse/TESTBOX-278)\] - Fix the coverage % in HTML visualizer

### New Features

* \[[TESTBOX-274](https://ortussolutions.atlassian.net/browse/TESTBOX-274)\] - New testbox output utilities struct: request.testbox
* \[[TESTBOX-276](https://ortussolutions.atlassian.net/browse/TESTBOX-276)\] - MockdataCFC is now a first class module in TestBox
* \[[TESTBOX-277](https://ortussolutions.atlassian.net/browse/TESTBOX-277)\] - New mockData\(\) method in your base specs so you can mock any type of data
* \[[TESTBOX-280](https://ortussolutions.atlassian.net/browse/TESTBOX-280)\] - Add cfconfig.json for controlling output and consistency between testing in diff engines

### Tasks

* \[[TESTBOX-271](https://ortussolutions.atlassian.net/browse/TESTBOX-271)\] - Drop ACF11 Support
* \[[TESTBOX-273](https://ortussolutions.atlassian.net/browse/TESTBOX-273)\] - Drop old mxunit decorator for ORM Transaction Decorator

