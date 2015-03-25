# Specs

Specifications (Specs) are defined by calling the TestBox `it()` global function, which takes in a `title` and a `body` function/closure. The `title` is the title of this spec you will write and the `body` function/closure is a block of code that represents the spec. A spec will contain most likely one or more expectations that will test the state of the SUT (software under test) or sometimes referred to as code under test. In BDD style, your specifications are what is used to validate your requirements of a scenario which is your describe() block of your story.

|Argument|Required|Default|Type|Description|
|--|--|--|--|--|
|title|true|---|string|the title of the spec|
|body|true|---|closure/udf|The closure that represents the spec|
|labels|false|---|string/array|The list or array of labels this suite group belongs to|
|skip|false|false|Boolean|A flag or a closure that tells TestBox to skip this suite group from testing if true. If this is a closure it must return boolean.|

An expectation is a nice assertion DSL that TestBox exposes so you can pretty much read what should happen in the testing scenario. A spec will pass if all expectations pass. A spec with one or more expectations that fail will fail the entire spec.

```javascript
function run(){

     describe("A suite", function(){
          it("contains spec with an awesome expectation", function(){
               expect( true ).toBeTrue();
          });

            it("contains a spec with more than 1 expectation", function(){
               expect( [1,2,3] ).toBeArray();
               expect( [1,2,3] ).toHaveLength( 3 );
          });
     });

}
```

<h3 style="color:grey">They are closures Ma!</h3>

Since the implementations of the describe() and it() functions are closures, they can contain executable code that is necessary to implement the test. All ColdFusion rules of scoping apply to [closures](http://help.adobe.com/en_US/ColdFusion/10.0/Developing/WSe61e35da8d31851842acbba1353e848b35-8000.html), so please remember them. We recommend always using the variables scope for easy access and distinction.

```javascript
function run(){

     describe("A suite is a closure", function(){
          c = new Calculator();

          it("and so is a spec", function(){
               expect( c ).toBeTypeOf( 'component' );
          });
     });

}
```
