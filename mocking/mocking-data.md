# Mocking Data

## Introduction

Since version 4.x we have included as a dependency [MockDataCFC](https://forgebox.io/view/mockdatacfc).  MockDataCFC is a simple service to generate fake data as a JSON REST service, a ColdBox Module or a simple CFC Service API. The idea being that you may be offline, may not have access to an API, or simply need some fake data to test on your front end or seed a complete database with fake data.

**MockDataCFC** allows you to define the return data model in a very deterministic and simple modeling DSL. Read on 🚀 for some modeling goodness!

## mockData\(\)

Every test bundle has access to our mocking method: `mockData()` which internally calls the `mock()` method in the `MockDataCFC` module.  You will pass all the arguments that you need to produce beautiful mocked data.

## Available Mocking Types

The available types **MockDataCFC** supports are:

* `age`: Generates a random "adult" age of 18 to 75.
* `all_age`: Generates a random age of 1 to 100.
* `autoincrement` : Returns an incremented index starting from 1
* `baconlorem`: Returns bacon lorem ipsum text. If used as `baconlorem:N`, returns N paragraphs. If used as `baconlorem❌ Y`, returns a random number of paragraphs between X and Y.
* `date`: Generates a random date
* `datetime`: Generates a random date and time value
* `email`: Generates a random email.
* `fname`: Generates a random first name.
* `imageurl` : Generates a random image URL with a random protocol
* `imageurl_http` : Generates a random image URL with `http` only protocol
* `imageurl_https` : Generates a random image URL with `https` only protocol
* `ipaddress` : Generates an ipv4 address
* `name`: Generates a random name.
* `lname`: Generates a random last name.
* `lorem`: Returns lorem ipsum text. If used as `lorem:N`, returns N paragraphs. If used as `lorem❌ Y`, returns a random number of paragraphs between X and Y.
* `num`: By default, a number from 1 to 10. You can also use the form `num:X` for a random number between 1 and X. Or `num❌ Y` for a random number between X and Y.
* `oneof❌ y`: Requires you to pass N values after it delimited by a colon. Example: `oneof:male: female`. Will return a random value from that list.
* `rnd:N`, `rand:N`, `rnd❌ y`, `rand❌ y` : Generate random numbers with a specific range or range cap.
* `sentence`: Generates a sentences. If used as `sentence:N`, returns N sentences. If used as `sentence❌ Y`, returns a random number of sentences beetween X and Y.
* `ssn`: Generates a random Social Security number.
* `string`: Generates a random string of length 10 by default. You can increase the length by passing it `string:length`.
* `tel`: Generates a random \(American\) telephone number.
* `uuid`: Generates a random UUID
* `url` : Generates a random URL with a random protocol
* `url_http` : Generates a random URL with `http` only protocol
* `url_https` : Generates a random URL with `https` only protocol
* `website` : Generates a random website with random protocol
* `website_http` : Generates a random website, `http` only protocol
* `website_https` : Generates a random website, `https` only protocol
* `words`: Generates a single word. If used as `word:N`, returns N words. If used as `words❌ Y`, returns a random number of words beetween X and Y.

### Supplier Type \(Custom Data\)

You can also create your own content by using a supplier closure/lambda as your type. This is a function that will create the content and return it for you.

```javascript
"name" : function( index ){
	return "luis";
}

"name" : ( index ) => "luis";
}
```

The function receives the currently iterating `index` as an argument as well. All you need to do is return back content.

## Examples

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

