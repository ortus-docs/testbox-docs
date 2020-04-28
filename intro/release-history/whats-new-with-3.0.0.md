# What's New With 4.0.0



TestBox 4.0.0 is a major release.  It has compatibility changes that you should be aware and lots of good features!  

### Compatibility

The major compatibility issues are the engine support removals:

* Lucee 4.5 Support Dropped
* Adobe ColdFusion 11 Dropped

### Updating

It is easy to update, just type `update testbox` and you are done!

## Major Features

### TestBox Output Utilities

Sometimes you need to dump something that is in the CFC you are testing or maybe an asynchronous test. The `debug()` method is only accessible from your test bundle, so getting to the TestBox output utilities is not easy.  We have now implemented the testing utilities into the `request` scope as `request.testbox`

This utility struct provides you with the following methods:

| **Method** | **Comment** |
| :--- | :--- |
| `console()` | Send output to the console |
| `debug()` | Send output to the TestBox reporter debugger |
| `clearDebugBuffer()` | Clear the debugger |
| `print()` | Send output to the ColdFusion output buffer |
| `printLn()` | Same as print\(\) but adding a &lt;br&gt; separator |

This way in your code you can add them for better debugging, especially when testing async code:

```text
request.testbox.console( "I am here" )
request.testbox.debug( "why is this not running" )
```

### Mocking Data

We have included [MockDataCFC](https://www.forgebox.io/view/mockdatacfc) as a dependency to TestBox 4.  This will allow you to mock not only objects but data as well.  You can access the mocking method via the new `mockData()` method in all your specs.  This feature is a life-saver when mocking APIs or data within your applications.

```javascript
# Array of objects
var data = mockData(
    $num = 3,
    "author" = "name",
    "id" = "uuid"
);

# An object
var data = mockData(
    $returnType = "struct",
    "author" = "name",
    "id" = "uuid"
);
```

Let's imagine the following object graph:

```text
Author
    Has Many Books
        Has Many Categories
    Has Keywords
    Has A Publisher
```

I can then use this mocking DSL to define it:

```javascript
mockData(
    fullName    = "name",
    description = "sentence",
    age         = "age",
    id          = "uuid",
    createdDate = "datetime",
    isActive	= "oneof:true: false",

    // one to many complex object definitions
    books = [
        {
            $num = "rand:1:      3",
            "id" = "uuid",
            "title" = "words:1:      5",
            "categories" = {
                "$num"      = "2",
                "id"        = "uuid",
                "category"  = "words"
            }
        }
    ],

    // object definition
    publisher = {
        "id" 	= "uuid",
        "name" 	= "sentence"
    },

    // array of values
    keywords = [
        {
            "$num" 	= "rand:1:      10",
            "$type" = "words"
        }
    ]
);
```

{% hint style="info" %}
More information here: [https://www.forgebox.io/view/mockdatacfc](https://www.forgebox.io/view/mockdatacfc)
{% endhint %}

## Release Notes

### Bugs

* \[[TESTBOX-275](https://ortussolutions.atlassian.net/browse/TESTBOX-275)\] - Exception in `beforeTests`/`afterTests` not reported in a meaningful way on the ANT Junit Reporter
* \[[TESTBOX-278](https://ortussolutions.atlassian.net/browse/TESTBOX-278)\] - Fix the coverage % in HTML visualizer

### New Features

* \[[TESTBOX-274](https://ortussolutions.atlassian.net/browse/TESTBOX-274)\] - New testbox output utilities struct: request.testbox
* \[[TESTBOX-276](https://ortussolutions.atlassian.net/browse/TESTBOX-276)\] - MockdataCFC is now a first class module in TestBox
* \[[TESTBOX-277](https://ortussolutions.atlassian.net/browse/TESTBOX-277)\] - New mockData\(\) method in your base specs so you can mock any type of data
* \[[TESTBOX-280](https://ortussolutions.atlassian.net/browse/TESTBOX-280)\] - Add cfconfig.json for controlling output and consistency between testing in diff engines

### Tasks

* \[[TESTBOX-271](https://ortussolutions.atlassian.net/browse/TESTBOX-271)\] - Drop ACF11 Support
* \[[TESTBOX-273](https://ortussolutions.atlassian.net/browse/TESTBOX-273)\] - Drop old mxunit decorator for ORM Transaction Decorator

