---
description: Describe(), Feature(), Scenario(), Given(), When()
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-bdd-primer/suites-describe-your-tests
---

# Suites: Describe Your Tests

A test suite in TestBox is a collection of specifications that model what you want to test. As we will investigate, the way the suite is expressed can be of many different types.

> Test suite is a container that has a set of tests which helps testers in executing and reporting the test execution status.

A test suite begins with a call to our TestBox `describe()` function with at least two arguments: a `title` and a `body` function/closure. The `title` is the name of the suite to register and the `body` function/closure is the block of code that implements the suite.

When applying BDD to your tests, this function is used to describe your story scenarios that you will implement.

```javascript
function run( testResults, testBox ){

     describe("A suite", function(){
          it("contains spec with an awesome expectation", function(){
               expect( true ).toBeTrue();
          });
          it("contains spec with a failure expectation", function(){
               expect( true ).toBeFalse();
          });
     });

}
```

{% hint style="success" %}
The `describe()` function is also aliased with the following names:`story(), feature(), scenario(), given(), when()`
{% endhint %}

## Arguments

There are more arguments, which you can see below:



<table data-header-hidden><thead><tr><th>Argument</th><th width="128">Required</th><th width="90">Default</th><th width="127">Type</th><th>Description</th></tr></thead><tbody><tr><td>Argument</td><td>Required</td><td>Default</td><td>Type</td><td>Description</td></tr><tr><td><code>title</code></td><td>true</td><td>---</td><td>string</td><td>The title of the suite to register</td></tr><tr><td><code>body</code></td><td>true</td><td>---</td><td>closure/udf</td><td>The closure that represents the test suite</td></tr><tr><td><code>labels</code></td><td>false</td><td>---</td><td>string/array</td><td>The list or array of labels this suite group belongs to</td></tr><tr><td><code>asyncAll</code></td><td>false</td><td>false</td><td>Boolean</td><td>If you want to parallelize the execution of the defined specs in this suite group.</td></tr><tr><td><code>skip</code></td><td>false</td><td>false</td><td>Boolean</td><td>A flag or a closure that tells TestBox to skip this suite group from testing if true. If this is a closure it must return boolean.</td></tr></tbody></table>
