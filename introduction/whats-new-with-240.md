# What's New With 2.4.0

TestBox 2.4.0 is a minor release with some great new functionality and tons of fixes. This release has been a great community effort as many people in the community contributed to its release. Special thanks to Eric Peterson, Joe Gooch and Sean Corfield for their additions, testing and contributions.

## Release Notes
### Bugs

* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-169'>TESTBOX-169</a>] - discover if fail origin exists in errors and failures, else ignore as it causes issues
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-170'>TESTBOX-170</a>] - Custom reporter passed as CFC instance doesn&#39;t work ### New Feature
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-171'>TESTBOX-171</a>] - New mintext reporter
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-172'>TESTBOX-172</a>] - new matcher toBeJSON and new assertion isJSON
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-175'>TESTBOX-175</a>] - No need to pass method=runRemote anymore on spec runners, defaults now
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-176'>TESTBOX-176</a>] - Implements fluent API - addDirectory,addBundles,addDirectories on TestBox Core

### Improvements

* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-168'>TESTBOX-168</a>] - runRemote operations are not setting the default response to HTML, so wddx takes over
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-173'>TESTBOX-173</a>] - Add toSatisfy( predicate ) matcher
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-174'>TESTBOX-174</a>] - Add expectAll() to make it easier to work with collections

