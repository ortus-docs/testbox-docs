---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-bdd-primer/life-cycle-data-binding
---

# Life-Cycle Data Binding

You can pass in an argument called `data` , which is a `struct` of dynamic data, to all life-cycle methods.  This is useful when creating dynamic suites and specifications.  This `data` will then be passed into the executing body for each life-cycle method for you.

```javascript
beforeEach( 
    data = { mydata="luis" }, 
    body = function( currentSpec, data ){
        // The arguments.data is binded via the `data` snapshot above.
        data.myData == "luis";
    }
);
```

Here is a typical example:

```javascript
describe( "Ability to bind data to life-cycle methods", function(){

    var data = [
        "spec1",
        "spec2"
    ];

    for( var thisData in data ){
        describe( "Trying #thisData#", function(){

            beforeEach( 
                data : { myData = thisData }, 
                body : function( currentSpec, data ){
                    targetData = arguments.data.myData;
            });

            it( 
                title : "should account for life-cycle data binding", 
                data  : { myData = thisData },
                body  : function( data ){
                    expect( targetData ).toBe( data.mydata );
                }
            );

            afterEach( 
                data : { myData = thisData }, 
                body : function( currentSpec, data ){
                    targetData = arguments.data.myData;
            });
        });
    }

    for( var thisData in data ){

        describe( "Trying around life-cycles with #thisData#", function(){

            aroundEach( 
                data : { myData = thisData }, 
                body : function( spec, suite, data ){
                    targetData = arguments.data.myData;
                    arguments.spec.body( data=arguments.spec.data );
            });

            it( 
                title : "should account for life-cycle data binding", 
                data  : { myData = thisData },
                body  : function( data ){
                    expect(    targetData ).toBe( data.mydata );
            });

        });

    }
});
```
