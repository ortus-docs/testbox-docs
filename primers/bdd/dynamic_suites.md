# Dynamic Suites

With TestBox's BDD syntax, it is possible to create suites dynamically; however, there are a few things to be aware of.

Setup for *dynamic* suites must be done in the pseudo-constructor (versus in `beforeAll()`). This is because `variables`-scoped variables set in `beforeAll()` are not available in the `describe` closure (even though they *are* available in `it` closures). This behavior can be explained by the execution sequence of a BDD bundle: When the bundle's `run()` method is called, it *collects* preliminary test data via `describe`s. *After* preliminary test data are collected, the `beforeAll()` runs, followed by the `describe` closures.

Additionally, care must be taken to pass data into the `it` closures, otherwise strange behavior will result (the values from the last loop iteration will be repeated in the body of each looped `it`).

## Example

The following bundle creates suites dynamically, by looping over test metadata.

```javascript
component
	extends="testbox.system.BaseSpec"
	hint="This is an example of a TestBox BDD test bundle containing dynamically-defined suites."
{
	
	/*
	* Need to do config for *dynamic* test suites here, in the
	* pseudo-constructor, versus in `beforeAll()`.
	*/
	doDynamicSuiteConfig();
	
	/*
	* @hint This method is arbitrarily named, but it sets up 
	* metadata needed by the dynamic suites example. The setup
	* could have been done straight in the pseudo-constructor,
	* but it might be nice to organize it into such a method
	* as this.
	*/
	function doDynamicSuiteConfig(){
		variables.dynamicSuiteConfig = ["foo","bar","baz"];
	}
	
	function run( testResults, testBox ){

		/*
		* @hint Dynamic Test Suites Example
		*/
		// loop over test metadata
		for ( var thing in dynamicSuiteConfig ) {
			describe("Dynamic Suite #thing#", function(){
				// notice how data is passed into the it() closure:
				//  * data={keyA=valueA, keyB=ValueB}
				//  * function( data )
				it( title=thing & "test", data={thing=thing}, body=function( data ) {
					var thing = data.thing;
					expect(thing).toBe(thing);
				});
			});
		}
	
	}

}
```
