# What's New With 2.3.0

TestBox 2.3.0 is a minor release with some great new functionality and tons of fixes. This release has been a great community effort as many people in the community contributed to its release.

## Release Notes

 Bug

* \[[TESTBOX-123](https://ortussolutions.atlassian.net/browse/TESTBOX-123)\] - If test spec descriptor contains a comma, it can not be drilled down to run that one spec directly
* \[[TESTBOX-140](https://ortussolutions.atlassian.net/browse/TESTBOX-140)\] - Allow Mocking of an Interface that implements another interface
* \[[TESTBOX-158](https://ortussolutions.atlassian.net/browse/TESTBOX-158)\] - Give line number when an expectation fails or errors out

 New Feature

* \[[TESTBOX-150](https://ortussolutions.atlassian.net/browse/TESTBOX-150)\] - new expressive exception throwing goodness: $throws\(\)
* \[[TESTBOX-161](https://ortussolutions.atlassian.net/browse/TESTBOX-161)\] - Recursively call parent \`aroundEach\` functions in reverse tree format
* \[[TESTBOX-162](https://ortussolutions.atlassian.net/browse/TESTBOX-162)\] - Add annotation hooks for lifecycle methods
* \[[TESTBOX-163](https://ortussolutions.atlassian.net/browse/TESTBOX-163)\] - remove the TestBox tag contexts from the beginning of Failure Origins
* \[[TESTBOX-164](https://ortussolutions.atlassian.net/browse/TESTBOX-164)\] - Make test harness easier for development via CommandBox
* \[[TESTBOX-165](https://ortussolutions.atlassian.net/browse/TESTBOX-165)\] - Add travis build support for supporting pull requests and test matrixes
* \[[TESTBOX-166](https://ortussolutions.atlassian.net/browse/TESTBOX-166)\] - Update API Docs to leverage DocBox instead

 Improvement

* \[[TESTBOX-160](https://ortussolutions.atlassian.net/browse/TESTBOX-160)\] - Explicitly place the instance "scope" in the variables scope due to lucee full cascade support
* \[[TESTBOX-167](https://ortussolutions.atlassian.net/browse/TESTBOX-167)\] - update string buffers to string builders

