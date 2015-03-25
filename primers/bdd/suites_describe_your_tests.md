# Suites: Describe Your Tests

A test suite begins with a call to our TestBox `describe()` function with at least two arguments: a `title` and a `body` function/closure. The `title` is the name of the suite to register and the `body` function/closure is the block of code that implements the suite. When applying BDD to your tests, this function is used to describe your story scenarios that you will implement. There are more arguments which you can see below:

|Argument|Required|Default|Type|Description|
|--|--|--|--|--|
|title|true|---|string|The title of the suite to register|
|body|true|---|closure/udf|The closure that represents the test suite|
|labels|false|---|string/array|The list or array of labels this suite group belongs to|
|asyncAll|false|false|Boolean|If you want to parallelize the execution of the defined specs in this suite group.|
|skip|false|false|Boolean|A flag or a closure that tells TestBox to skip this suite group from testing if true. If this is a closure it must return boolean.|

```javascript
function run( testResults, testBox ){

     describe("A suite", function(){
          it("contains spec with an awesome expectation", function(){
               expect( true ).toBeTrue();
          });
     });

}
```


