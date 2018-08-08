# What's New With 2.5.0

TestBox 2.5.0 is a minor release with some great new functionality and tons of fixes. You can find the release notes here and the major updates for this release. One of the biggest features for TestBox that was not part of TestBox, was the addition of TestBox Watchers to CommandBox.

## TestBox Watchers

CommandBox CLI has added watching capabilities to the `testbox` namespace. Which means you can now run the following command in the root of your project: `testbox watch` and it will monitor all your CFCs for changes. If a change is detected, then it will fire the new `testbox run` command with our CLI reporter.

## Life-Cycle Data Binding

We had introduced spec data binding in TestBox for creating dynamic specs and passing dynamic data into them. We have extended this feature into life-cycle methods:

* `beforeEach()`
* `afterEach()`
* `aroundEach()`

You can now pass in an argument called `data` which is a struct of dynamic data to pass into the life-cycle method. You can then pickup this data in the closure for the life-cycle. Here is a typical example:

```javascript
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
                    expect(    targetData ).toBe( data.mydata );
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
                    expect(    targetData ).toBe( data.mydata );
                }
            );

        });

    }
});
```

## Release Notes

### Bugs

* \[[TESTBOX-177](https://ortussolutions.atlassian.net/browse/TESTBOX-177)\] - Simple reporter broken on Adobe servers
* \[[TESTBOX-180](https://ortussolutions.atlassian.net/browse/TESTBOX-180)\] - HTML Runner links should include directory param
* \[[TESTBOX-181](https://ortussolutions.atlassian.net/browse/TESTBOX-181)\] - AsyncAll Errroring on xUnit runner
* \[[TESTBOX-183](https://ortussolutions.atlassian.net/browse/TESTBOX-183)\] - the 'type' argument was being ignored if the 'regex' argument was provided and matched the exception data          

### Improvements

* \[[TESTBOX-178](https://ortussolutions.atlassian.net/browse/TESTBOX-178)\] - Remove thread scope from debug stream
* \[[TESTBOX-184](https://ortussolutions.atlassian.net/browse/TESTBOX-184)\] - Passing Data to Dynamic Tests via life-cycle methods: around, after, before
* \[[TESTBOX-185](https://ortussolutions.atlassian.net/browse/TESTBOX-185)\] - Preserve whitespace in HTML \(simple\) Reporter
* \[[TESTBOX-186](https://ortussolutions.atlassian.net/browse/TESTBOX-186)\] - Include TestBox version in TestResult memento
* \[[TESTBOX-187](https://ortussolutions.atlassian.net/browse/TESTBOX-187)\] - Let travis handle upload of api docs and snapshot binaries
* \[[TESTBOX-188](https://ortussolutions.atlassian.net/browse/TESTBOX-188)\] - Aggregate suite stats from nested suties

