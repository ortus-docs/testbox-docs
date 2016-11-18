# What's New With 2.4.0

TestBox 2.4.0 is a minor release with some great new functionality and tons of fixes. This release has been a great community effort as many people in the community contributed to its release. Special thanks to Eric Peterson, Joe Gooch and Sean Corfield for their additions, testing and contributions.

## New `toSatisfy` matcher

This new matcher is thanks to Sean Corfield.  It allows you to create your own closure that will evaluate the expectation and then decide if it passes the given truth test.

```js
it( "can satisfy truth tests", function(){

    expect( 1 ).toSatisfy( function( num ){ return arguments.num > 0; } );

    expect( 0 ).notToSatisfy( function( num ){ return arguments.num > 0; } );

});

```

## New `expectAll()` collection expectation

Sean was busy in this release and provided us with this awesome feature in which you can call on a new `expectAll()` and pass either an array or struct.  TestBox will then iterate for you and call all the chained matchers upon the collection items.

```js
it( "can test a collection", function(){
    expectAll( [2,4,6,8] ).toSatisfy( function(x){ return 0 == x%2; });
    expectAll( {a:2,b:4,c:6} ).toSatisfy( function(x){ return 0 == x%2; });

    // and we can chain matchers
    expectAll( [2,4,6,8] )
        .toBeGTE( 2 )
        .toBeLTE( 8 );
});

```

## New `mintext` Reporter
This new reporter is to enhance console based runners in order for the report to be more legible.

## New JSON Matchers & Assertions

You can now use a `toBeJSON()` matcher or a `$assert.isJSON` assertion.

## No more `runRemote`
You no longer need to pass `?method=runRemote` in the URL when executing a test bundle via the URL.  This will automatically be added for you.

## New Fluent API for Testing Declarations

Thanks to Joe Gooch you can now use the new methods in the TestBox cfc
* `addDirectory()`
* `addDirectories()`
* `addBundles()`

You can chain them as you see fit and they will aggregate the specs collected.

## Release Notes

Here is the full release notes for this release
### Bugs

* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-169'>TESTBOX-169</a>] - discover if fail origin exists in errors and failures, else ignore as it causes issues
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-170'>TESTBOX-170</a>] - Custom reporter passed as CFC instance doesn&#39;t work 

### New Features
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-171'>TESTBOX-171</a>] - New mintext reporter
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-172'>TESTBOX-172</a>] - new matcher toBeJSON and new assertion isJSON
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-175'>TESTBOX-175</a>] - No need to pass method=runRemote anymore on spec runners, defaults now
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-176'>TESTBOX-176</a>] - Implements fluent API - addDirectory,addBundles,addDirectories on TestBox Core

### Improvements

* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-168'>TESTBOX-168</a>] - runRemote operations are not setting the default response to HTML, so wddx takes over
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-173'>TESTBOX-173</a>] - Add toSatisfy( predicate ) matcher
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-174'>TESTBOX-174</a>] - Add expectAll() to make it easier to work with collections

