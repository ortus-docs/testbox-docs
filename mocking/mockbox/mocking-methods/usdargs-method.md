---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/mocking/mockbox/mocking-methods/usdargs-method
---

# $args() Method

This method is used to tell MockBox that you want to mock a method with a SPECIFIC number of argument calls. Then you will have to set the return results for it, but this is absolutely necessary if you need to test an object that makes several method calls to the same method with different arguments, and you need to mock different results coming back. Example, let's say you are using a ColdBox configuration bean that holds configuration data. You make several calls to the `getKey()` method with different arguments:

```javascript
configBean.getKey('DebugMode');
configBean.getKey('OutgoingMail');
```

How in the world can I mock this? Well, using the mock arguments method.

```javascript
//get a mock config bean
mockConfig = getMockBox().createEmptyMock("coldbox.system.beans.ConfigBean");
//mock the method for positional arguments
mockConfig.$("getKey").$args("debugmode").$results(true);
mockConfig.$("getKey").$args("OutgoingMail").$results('devmail@mail.com');

//Then you can call and get the expected results
```

> **Hint** So remember that if you use the `$args()` call, you need to tell it what kind of results you are expecting by calling the `$results()` method after it or you might end up with an exception.

If the method you are mocking is called using named arguments then you can mock this using:

```javascript
//get a mock config bean
mockConfig = getMockBox().createEmptyMock("coldbox.system.beans.ConfigBean");
//mock the method for named arguments
mockConfig.$("getKey").$args(name="debugmode").$results(true);
```

## Matching Complex Arguments

`$args()` matches structurally, so a struct argument matches whatever order its keys were built in:

```javascript
mockService.$( "charge" )
    .$args( { amount : 100, currency : "USD" } )
    .$results( true );

// matches, despite the different key order at the call site
mockService.charge( { currency : "USD", amount : 100 } );
```

{% hint style="warning" %}
Before TestBox 7.1, nested structures were hashed in a way that depended on struct iteration order, so two structurally-equal structs built in a different order could fail to match and the mock would return `null` instead. This was always latent but became reproducible on Lucee 7.1, which changed its underlying map implementation. Upgrade to 7.1 or later if you mock methods that take struct arguments.
{% endhint %}

As of TestBox 7.1, `$args()` also understands BoxLang `Set` and `Range` objects when matching:

```javascript
mockService.$( "grant" )
    .$args( setOf( "admin", "editor" ) )
    .$results( true );

mockService.$( "paginate" )
    .$args( 1..10 )
    .$results( results );
```
