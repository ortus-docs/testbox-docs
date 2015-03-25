# What's New With 2.1.0

TestBox 2.1.0 is a minor release with some great new functionality and tons of fixes.

## Release Notes
<h3>Bugs Fixed
</h3>
<ul>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-96" rel="nofollow">TESTBOX-96</a>] - isEqual on Query fails when queries are equal</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-97" rel="nofollow">TESTBOX-97</a>] - equalize fails on struct/objects/arrays when null values exist within them</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-98" rel="nofollow">TESTBOX-98</a>] - Floating Point Number isEqual Fails</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-100" rel="nofollow">TESTBOX-100</a>] - Specs with the same name cause thread name exceptions when using async</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-101" rel="nofollow">TESTBOX-101</a>] - Download file has &#34;samples&#34; instead of &#34;tests&#34; directory</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-102" rel="nofollow">TESTBOX-102</a>] - tobe() cannot handle sparse arrays on Adobe CF</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-103" rel="nofollow">TESTBOX-103</a>] - xUnit compatibility CF9 broken due to isClosure() being utilized</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-105" rel="nofollow">TESTBOX-105</a>] - skip closures get more metadata arguments when being executed.</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-115" rel="nofollow">TESTBOX-115</a>] - testbox errors when using complete null support in railo</li>
</ul>
<h3> Improvement
</h3>
<ul>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-40" rel="nofollow">TESTBOX-40</a>] - Have debug() include information about where it came from</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-95" rel="nofollow">TESTBOX-95</a>] - remove extra whitespace in text reporter</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-110" rel="nofollow">TESTBOX-110</a>] - Remove CF7,8 incompatibilities</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-118" rel="nofollow">TESTBOX-118</a>] - ColdFusion 11 cfinclude compatibilities</li>
</ul>
<h3> New Feature
</h3>
<ul>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-106" rel="nofollow">TESTBOX-106</a>] - BDD run() method now receive the TestResults argument for usage in their definitions.</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-107" rel="nofollow">TESTBOX-107</a>] - BDD runner and specs receive reference to the TestBox calling class via the run() method</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-108" rel="nofollow">TESTBOX-108</a>] - Update the apidocs with our new DocBox skin</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-109" rel="nofollow">TESTBOX-109</a>] - Debug labels and telemetry additions</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-112" rel="nofollow">TESTBOX-112</a>] - Add &#34;top&#34; attribute to debug method</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-114" rel="nofollow">TESTBOX-114</a>] - HTMLRunner add big request timeout setting to avoid server cut offs </li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-116" rel="nofollow">TESTBOX-116</a>] - have expectations assertions return the expectation to allow chaining</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-117" rel="nofollow">TESTBOX-117</a>] - Simple reporter includes now a test bundle filter</li>
<li>[<a href="https://ortussolutions.atlassian.net/browse/TESTBOX-119" rel="nofollow">TESTBOX-119</a>] - New lifecycle method: aroundEach() so you can do a full AOP advice on any spec</li>
</ul>

## Global Enhancements
* Better whitespace management on text enabled reporting
* ColdFusion 11 compatibilities
* ColdFusion 9 support on xUnit and Assertions
* New searchable API Docs with new skin: http://apidocs.ortussolutions.com/testbox/2.1.0/index.html
* HTML Runners now have a high request timeout for long-lasting tests

## Debugging Enhancements
TestBox's `debug()` method has been enhanced to provide greater messages and telemetry. You can even control the depth of the dumps of each of the debugged data as it is sent. Here is the new signature:

```js
/**
* Debug some information into the TestBox debugger array buffer
* @var The data to debug
* @label The label to add to the debug entry
* @deepCopy By default we do not duplicate the incoming information, but you can :)
* @top The top numeric number to dump on the screen in the report, defaults to 999
*/
any function debug(
	any var,
	string label="",
	boolean deepCopy=false,
	numeric top="999"
)
```

The output will now include information as to what spec produced the output, timestamps, data, and thread data.