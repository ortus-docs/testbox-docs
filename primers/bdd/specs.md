# Specs

Specifications (Specs) are defined by calling the TestBox `it()` global function, which takes in a `title` and a `body` function/closure. The `title` is the title of this spec you will write and the `body` function/closure is a block of code that represents the spec. A spec will contain most likely one or more expectations that will test the state of the [SUT](http://en.wikipedia.org/wiki/System_under_test) (software under test) or sometimes referred to as code under test. In BDD style, your specifications are what is used to validate your requirements of a scenario which is your `describe()` block of your [story](http://en.wikipedia.org/wiki/User_story).

|Argument|Required|Default|Type|Description|
|--|--|--|--|--|
|title|true|---|string|the title of the spec|
|body|true|---|closure/udf|The closure that represents the spec|
|labels|false|---|string/array|The list or array of labels this suite group belongs to|
|skip|false|false|Boolean|A flag or a closure that tells TestBox to skip this suite group from testing if true. If this is a closure it must return boolean.|
|data|false|`{}`|struct | A struct of data you can bind the spec with so you can use within the `body` closure|

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

##They are closures Ma!

Since the implementations of the `describe()` and `it()` functions are closures, they can contain executable code that is necessary to implement the test. All CFML rules of scoping apply to [closures](http://help.adobe.com/en_US/ColdFusion/10.0/Developing/WSe61e35da8d31851842acbba1353e848b35-8000.html), so please remember them. We recommend always using the `variables` scope for easy access and distinction.

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

## Spec Data Binding

The `data` argument can be used to pass in a struct of data into the spec so it can be used later within the body closure.  This is great when doing looping and dynamic closure calls:

```javascript
// Simple Example
it( title="can handle binding", body=function( data ){
	expect(	data.keep ).toBeTrue();
}, data = { keep = true } );

// Complex Example
for (filePath in files) {
	it("#getFileFromPath(filePath)# should be valid JSON", function() {
		var json = fileRead(filePath);
		var isItJson = isJSON(json);
		expect(json).notToBeEmpty();
		expect(isItJson).toBeTrue();
		if (isItJson) {
			var data = deserializeJSON(json);
			if (getFileFromPath(filePath) != "index.json") {
				expect(data).toHaveKey("name");
				expect(data).toHaveKey("type");
			}
		}
		
	});
}
```