# Custom Reporters


## Building Reporters

You can also build your own reporters by implementing our core class: `testbox.system.reporters.IReport`

```javascript
interface{

  /**
  * Get the name of the reporter
  */
  function getName();

  /**
  * Do the reporting thing here using the incoming test results
  * The report should return back in whatever format they desire and should set any
  * Specifc browser types if needed.
  * @results.hint The instance of the TestBox TestResult object to build a report on
  * @testbox.hint The TestBox core object
  * @options.hint A structure of options this reporter needs to build the report with
  */
  any function runReport(
    required testbox.system.TestResult results,
    required testbox.system.TestBox testbox,
    struct options={} );
}
```

## Executing Your Reporter

Once you implement your own report you just need to pass the class path or the instance of your reporter to the TestBox runner methods using the `reporter` argument.  The `reporter` argument can be the following values:

* `string` - The class path of your reporter
* `instance` - The instance of your reporter CFC
* `struct` - A structure representing your reporter with the following keys: `{ type="class_path", options={}`. This is mostly used if you want to instantiate and use your reporter with a structure of options.

Now you can init TestBox with your reporter:

```javascript
r = new TestBox( reporter="my.path.toCustomReporter" );
r = new TestBox( reporter= new my.path.CustomReporter() );
r = new TestBox( reporter={
    type = "my.path.to.CustomReporter",
    options = { name = value, name2 = value2 }
} );
```

## Sample Reporter

Here is a sample reporter for you that generates JSON for the output.

```javascript
component{

  function init(){ return this; }

  /**
  * Get the name of the reporter
  */
  function getName(){
    return "JSON";
  }

  /**
  * Do the reporting thing here using the incoming test results
  * The report should return back in whatever format they desire and should set any
  * Specifc browser types if needed.
  * @results.hint The instance of the TestBox TestResult object to build a report on
  * @testbox.hint The TestBox core object
  * @options.hint A structure of options this reporter needs to build the report with
  */
  any function runReport(
    required testbox.system.TestResult results,
    required testbox.system.TestBox testbox,
    struct options={}
  ){
    getPageContext().getResponse().setContentType( "application/json" );
    return serializeJSON( arguments.results.getMemento() );
  }

}
```

