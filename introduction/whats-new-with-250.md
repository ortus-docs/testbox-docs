# What's New With 2.5.0

TestBox 2.5.0 is a minor release with some great new functionality and tons of fixes.  You can find the release notes here and the major updates for this release. One of the biggest features for TestBox that was not part of TestBox, was the addition of TestBox Watchers to CommandBox.

## TestBox Watchers

CommandBox CLI has added watching capabilities to the `testbox` namespace.  Which means you can now run the following command in the root of your project: `testbox watch` and it will monitor all your CFCs for changes.  If a change is detected, then it will fire the new `testbox run` command with our CLI reporter.

## Life-Cycle Data Binding
We had introduced spec data binding in TestBox for creating dynamic specs and passing dynamic data into them.  We have extended this feature into life-cycle methods:

* `beforeEach()`
* `afterEach()`
* `aroundEach()`

You can now pass in an argument called `data` which is a struct of dynamic data to pass into the life-cycle method.  You can then pickup this data in the closure for the life-cycle. 

I tend to (want to) do a lot of dynamic tests, to keep DRY. I'm at a roadblock now because I can't get outside data into lifecycle methods (e.g., {{beforeEach}}). I know I can pass in outside data to an {{it}}, because it's set up to handle that, but I need it in {{beforeEach}}.

https://gist.github.com/jamiejackson/9874cf01e4fd04931e6150b3675d5cc2#file-needdatainbeforeeach-cfc-L193

h2. Implementation Below:

```js
describe( "Ability to bind data to life-cycle methods", function(){
			
			var data = [
				"spec1",
				"spec2"
			];

			for( var thisData in data ){
				describe( "Trying #thisData#", function(){
					
					beforeEach( data={ myData = thisData }, body=function( currentSpec, data ){
						targetData = arguments.data.myData;
					});
					
					it( title="should account for life-cycle data binding", 
						data={ myData = thisData},
						body=function( data ){
							expect(	targetData ).toBe( data.mydata );
						}
					);

					afterEach( data={ myData = thisData }, body=function( currentSpec, data ){
						targetData = arguments.data.myData;
					});
				});
			}

			for( var thisData in data ){

				describe( "Trying around life-cycles with #thisData#", function(){
					
					aroundEach( data={ myData = thisData }, body = function( spec, suite, data ){
						targetData = arguments.data.myData;
						arguments.spec.body( data=arguments.spec.data );
					});

					it( title="should account for life-cycle data binding", 
						data={ myData = thisData },
						body=function( data ){
							expect(	targetData ).toBe( data.mydata );
						}
					);
				
				});

			}
		});
```

## Release Notes    

### Bugs
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-177'>TESTBOX-177</a>] - Simple reporter broken on Adobe servers
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-180'>TESTBOX-180</a>] - HTML Runner links should include directory param
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-181'>TESTBOX-181</a>] - AsyncAll Errroring on xUnit runner
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-183'>TESTBOX-183</a>] - the &#39;type&#39; argument was being ignored if the &#39;regex&#39; argument was provided and matched the exception data          

### Improvements
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-178'>TESTBOX-178</a>] - Remove thread scope from debug stream
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-184'>TESTBOX-184</a>] - Passing Data to Dynamic Tests via life-cycle methods: around, after, before
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-185'>TESTBOX-185</a>] - Preserve whitespace in HTML (simple) Reporter
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-186'>TESTBOX-186</a>] - Include TestBox version in TestResult memento
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-187'>TESTBOX-187</a>] - Let travis handle upload of api docs and snapshot binaries
* [<a href='https://ortussolutions.atlassian.net/browse/TESTBOX-188'>TESTBOX-188</a>] - Aggregate suite stats from nested suties
