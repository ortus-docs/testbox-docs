---
icon: check-double
metaLinks:
  alternates:
    - https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/digging-deeper/expectations
---

# Expectations

TestBox allows you to create BDD expectations with our expectations and matcher API DSL. You start by calling our `expect()` method, usually with an **actual** value you would like to test. You then concatenate the expectation of that actual value/function to a result or what we call a matcher. You can also concatenate matchers (as of v2.1.0) so you can provide multiple matching expectations to a single value.

```javascript
expect( 43 ).toBe( 42 );
expect( () => calculator.add(2,2) ).toThrow();
```

## Adding Context To Failures

When several expectations in a spec assert against similar values, a raw matcher failure such as `expected [100] to be [108]` does not tell you which one broke. `withContext()` attaches a semantic label that is prepended to the failure message.

{% tabs %}
{% tab title="BoxLang" %}
```java
expect( order.getSubtotal() ).withContext( "subtotal" ).toBe( 100 )
expect( order.getTotal() ).withContext( "total after tax" ).toBe( 108 )

// Failure: total after tax: expected [100] to be [108]
```
{% endtab %}

{% tab title="CFML" %}
```cfscript
expect( order.getSubtotal() ).withContext( "subtotal" ).toBe( 100 );
expect( order.getTotal() ).withContext( "total after tax" ).toBe( 108 );

// Failure: total after tax: expected [100] to be [108]
```
{% endtab %}
{% endtabs %}

The context flows through standard matchers, negated matchers and [custom matchers](custom-matchers.md) alike.

{% hint style="success" %}
`withContext()` shines in loops and data-driven specs, where the same expectation runs many times and the failure message alone cannot identify the iteration.
{% endhint %}

## Collection Expectations

Instead of looping and expecting per element, you can assert against an entire array or struct at once. The matcher you chain is applied to every element, and the mode decides what has to pass.

| Mode | Passes when |
| --- | --- |
| `expectAll( collection )` | Every element satisfies the matcher |
| `expectAny( collection )` | At least one element satisfies the matcher |
| `expectSome( collection, min, max )` | Between `min` and `max` elements satisfy the matcher |
| `expectNone( collection )` | No element satisfies the matcher |

{% tabs %}
{% tab title="BoxLang" %}
```java
// every user must have an id
expectAll( users ).toHaveKey( "id" )

// at least one order is over the free-shipping threshold
expectAny( orders ).toBeGT( 50 )

// between 2 and 5 of them are flagged
expectSome( flags, 2, 5 ).toBeTrue()

// no serialized user may carry a password
expectNone( serializedUsers ).toHaveKey( "password" )
```
{% endtab %}

{% tab title="CFML" %}
```cfscript
// every user must have an id
expectAll( users ).toHaveKey( "id" );

// at least one order is over the free-shipping threshold
expectAny( orders ).toBeGT( 50 );

// between 2 and 5 of them are flagged
expectSome( flags, 2, 5 ).toBeTrue();

// no serialized user may carry a password
expectNone( serializedUsers ).toHaveKey( "password" );
```
{% endtab %}
{% endtabs %}

Failure messages report the pass and fail counts plus per-element detail, including the array index or struct key of each failing element, so you learn which elements failed rather than only that something did.

```
expectAll: 3 of 5 elements passed
  [2] expected [null] to have key [id]
  [5] expected [null] to have key [id]
```
