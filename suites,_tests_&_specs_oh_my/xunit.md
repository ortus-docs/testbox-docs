# xUnit Tests

The testing bundle CFC is actually the suite in xUnit style as it contains all the test methods you would like to test with. Usually, this CFC represents a test case for a specific software under test (SUT), whether that's a model object, service, etc. This component can have some cool annotations as well that can alter its behavior.


```javascript
component displayName="The name of my suite" asyncAll="boolean" labels="list" skip="boolean"{

}
```

##Bundle Annotations

|Argument|Required|Default|Type|Description|
|--|--|--|--|--|
|displayName |false|--|string|If used, this will be the name of the test suite in the reporters.|
|asyncAll|false|false|boolean|If true, it will execute all the test methods in parallel and join at the end asynchronously.|
|labels|false|---|string/list|The list of labels this test belongs to|
|skip|false|false|boolean/udf|A boolean flag that makes the runners skip the test for execution. It can also be the name of a UDF in the same CFC that will be executed and MUST return a boolean value.|

> **Caution** If you activate the `asyncAll` flag for asynchronous testing, you HAVE to make sure your tests are also thread safe and appropriately locked.

##Tests
TestBox discovers test methods in your bundle CFC by applying the following discovery rules:

* Any method that has a `test` annotation on it
* Any public method that starts or ends with the word `test`

```javascript
// Via inline annotation
function shouldBeAwesome() test{}

/**
* Via comment annotation
* @test
*/
function shouldBeAwesome(){}

// via conventions
function testShouldDoThis(){}
function shouldDoThisTest(){}
```

Each test method will test the state of the SUT (software under test) or sometimes referred to as code under test. It will do so by asserting that actual values from an execution match an expected value or condition. TestBox offers an assertion library that you have available in your bundle via the injected variable `$assert`. You can also use our expectations library if you so desire, but that is mostly used in our BDD approach (chapter 3: [Primers](../primers/index.md)).

```javascript
function testIncludes(){
       $assert.includes( "hello", "HE" );
       $assert.includes( [ "Monday", "Tuesday" ] , "monday" );
}
```

Each test function can also have some cool annotations attached to it.

|Arguments|Required|Default|Type|Description|
|--|--|--|--|--|
| `labels` |false|string/list|---|The list of labels this test belongs to|
| `skip` |false|false|boolean/udf|A boolean flag that makes the runners skip the test for execution. It can also be the name of a UDF in the same CFC that will be executed and MUST return a boolean value.|

