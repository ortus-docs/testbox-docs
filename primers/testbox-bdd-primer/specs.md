# Specs

A spec is a declaration that will usually test your system with a requirement.  They are defined by calling the TestBox `it()` global function, which takes in a `title` and a `body` function/closure. The `title` is the title of this spec you will write and the `body` function/closure is a block of code that represents the spec. 

A spec will contain most likely one or more expectations that will test the state of the [SUT](http://en.wikipedia.org/wiki/System_under_test) \(software under test\) or sometimes referred to as code under test. In BDD style, your specifications are what is used to validate your requirements of a scenario which is your `describe()` block of your [story](http://en.wikipedia.org/wiki/User_story).

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

An expectation is a nice assertion DSL that TestBox exposes so you can pretty much read what should happen in the testing scenario. A spec will pass if all expectations pass. A spec with one or more expectations that fail will fail the entire spec.

{% hint style="success" %}
The `it()` function is also aliased as `then()`
{% endhint %}

### Arguments

| Argument | Required | Default | Type | Description |
| :--- | :--- | :--- | :--- | :--- |
| title | true | --- | string | the title of the spec |
| body | true | --- | closure/udf | The closure that represents the spec |
| labels | false | --- | string/array | The list or array of labels this suite group belongs to |
| skip | false | false | Boolean | A flag or a closure that tells TestBox to skip this suite group from testing if true. If this is a closure it must return boolean. |
| data | false | `{}` | struct | A struct of data you can bind the spec with so you can use within the `body` closure |

## They are closures Ma!

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

The `data` argument can be used to pass in a structure of data into the spec so it can be used later within the body closure. This is great when doing looping and creating dynamic closure calls:

```javascript
// Simple Example
it( title="can handle binding", body=function( data ){
    expect(    data.keep ).toBeTrue();
}, data={ keep = true } );

// Complex Example. Let's iterate over a bunch of files and create dynamic specs
for( var filePath in files ){

  it( 
    title="#getFileFromPath( filePath )# should be valid JSON", 
    // pass in a struct of data to the spec for later evaluation
    data={ filePath = filePath },
    // the spec closure accepts the data for later evaluation
    body=function( data ) {
      var json = fileRead( data.filePath );
      var isItJson = isJSON( json );

      expect( json ).notToBeEmpty();
      expect( isItJson ).toBeTrue();

      if( isItJson ){
          var jsonData = deserializeJSON(json);
          if( getFileFromPath( filePath ) != "index.json"){
              expect( jsonData ).toHaveKey( "name" );
              expect( jsonData ).toHaveKey( "type" );
          }
      }

  });
}
```

