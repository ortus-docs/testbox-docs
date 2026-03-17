---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-xunit-primer/bundles-group-your-tests
---

# Test Bundles

The testing bundle CFC is actually the suite in xUnit style as it contains all the test methods you would like to test with. Usually, this CFC represents a test case for a specific software under test (SUT), whether that's a model object, service, etc. This component can have some cool annotations as well that can alter its behavior.

```cfscript
component displayName="The name of my suite" asyncAll="boolean" labels="list" skip="boolean"{

}
```



TestBox relies on the fact of creating testing bundles which are basically CFCs. A bundle CFC will hold all the tests the TestBox runner will execute and produce reports on. Thus, sometimes this test bundle is referred to as a test suite in xUnit terms.

```javascript
component displayName="My test suite" extends="testbox.system.BaseSpec"{

     // executes before all tests
     function beforeTests(){}

     // executes after all tests
     function afterTests(){}

}
```

### Bundle Annotations <a href="#bundle-annotations" id="bundle-annotations"></a>

| Argument    | Required | Default | Type        | Description                                                                                                                                                                |
| ----------- | -------- | ------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| displayName | false    | --      | string      | If used, this will be the name of the test suite in the reporters.                                                                                                         |
| asyncAll    | false    | false   | boolean     | If true, it will execute all the test methods in parallel and join at the end asynchronously.                                                                              |
| labels      | false    | ---     | string/list | The list of labels this test belongs to                                                                                                                                    |
| skip        | false    | false   | boolean/udf | A boolean flag that makes the runners skip the test for execution. It can also be the name of a UDF in the same CFC that will be executed and MUST return a boolean value. |

> **Caution** If you activate the `asyncAll` flag for asynchronous testing, you HAVE to make sure your tests are also thread safe and appropriately locked.

### &#x20; <a href="#tests" id="tests"></a>

<br>
