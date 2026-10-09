---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-xunit-primer/test-methods
---

# Test Methods

TestBox discovers test methods in your bundle CFC by applying the following discovery rules:

* Any method that has a test annotation on it
* Any public method that starts or ends with the word test

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

Each test method will test the state of the SUT (software under test) or sometimes referred to as code under test. It will do so by asserting that actual values from an execution match an expected value or condition. TestBox offers an assertion library that you have available in your bundle via the injected variable $assert. You can also use our expectations library if you so desire, but that is mostly used in our BDD approach.

```javascript
function testIncludes(){
       $assert.includes( "hello", "HE" );
       $assert.includes( [ "Monday", "Tuesday" ] , "monday" );
}
```

Each test function can also have some cool annotations attached to it.

| Argument | Required | Default | Type        | Description                                                                                                                                                                |
| -------- | -------- | ------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| labels   | false    | ---     | string/list | The list of labels this test belongs to                                                                                                                                    |
| skip     | false    | false   | boolean/udf | A boolean flag that makes the runners skip the test for execution. It can also be the name of a UDF in the same CFC that will be executed and MUST return a boolean value. |
| retries  | false    | 0       | numeric     | How many extra times to run the test when it fails or errors, with its `setup()` and `teardown()`. `0` inherits the bundle `retries` annotation or the global `retries` option. See [Retries](../../browser-testing/retries.md). |

{% tabs %}
{% tab title="BoxLang" %}
```groovy
@retries( 2 )
function testChargesACardInTheSandbox() {
    $assert.isEqual( "succeeded", gateway.charge( 100 ).status )
}
```
{% endtab %}

{% tab title="CFML" %}
```cfscript
function testChargesACardInTheSandbox() retries="2"{
    $assert.isEqual( "succeeded", gateway.charge( 100 ).status );
}
```
{% endtab %}
{% endtabs %}

A test method can also attach files, such as screenshots or logs, to its result with `attach( path, type, name )`. See [Attachments](../../browser-testing/attachments.md).
