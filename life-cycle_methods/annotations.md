# Annotations

In addition to the life-cycle methods, you can make any method a life-cycle method by giving it the desired annotation in its function definition.  This is especially useful for parent classes that want to hook in to the TestBox life-cycle.

* `@beforeAll` - Executes once before all specs for the entire test bundle CFC
* `@afterAll` - Executes once after all specs complete in the test bundle CFC
* `@beforeEach` - Executes before every single spec in a single describe block and receives the currently executing spec.
* `@afterEach` - Executes after every single spec in a single describe block and receives the currently executing spec.
* `@aroundEach` - Executes around the executing spec so you can provide code surrounding the spec.

Below are several examples using script notation.

**DBTestCase.cfc (parent class)**

```javascript
component extends="coldbox.system.testing.BaseTestCase"{

    /**
     * @aroundEach
     */
    function wrapInDBTransaction( spec, suite ){
        transaction action="begin" {
            try {
                arguments.spec.body();
            } catch (any e) {
                rethrow;
            } finally {
                transaction action="rollback"
            }
        }
     }
}
```

**PostsTest.cfc**

```javascript
component extends="DBTestCase"{

    /**
     * @beforeEach
     */
    function setupColdBox() {
        setup();
    }

    function run() {
        given( "I have a two posts", function(){
            when( "I visit the home page", function(){
                then( "There should be two posts on the page", function(){
                    queryExecute( "INSERT INTO posts (body) VALUES ('Test Post One')" );
                    queryExecute( "INSERT INTO posts (body) VALUES ('Test Post Two')" );

                    var event = execute( event = "main.index", renderResults = true );

                    var content = event.getCollection().cbox_rendered_content;

                    expect(content).toMatch( "Test Post One" );
                    expect(content).toMatch( "Test Post Two" );
                });
            });
        });
    }
}

```

This also helps parent classes enforce their setup methods are called by annotating the methods with `@beforeAll`.  No more forgetting to call `super.beforeAll()`!

You can have as many annotated methods as you would like.  TestBox discovers them up the inheritance chain and calls them in reverse order.
