---
description: Rerun failing or erroring specs a few times before reporting them, per spec, per bundle or for the whole run.
icon: arrows-rotate
---

# Retries

Browser tests talk to a real browser, a real server and sometimes real third-party services, so a spec can fail once for reasons that have nothing to do with your code. **Retries** run a failing or erroring spec again, up to a number of extra times, and report the final attempt. They work for every spec, browser or not, BDD or xUnit, on every engine.

{% hint style="success" %}
Retries hide flakiness, they do not fix it. Prefer the web-first [browser matchers](browser-matchers.md), which already wait, over retries, and keep the retry count low. The [attempts](#attempts-in-the-results) TestBox records tell you which specs need attention.
{% endhint %}

## How a Retry Runs

When an attempt fails or errors and attempts remain, TestBox runs the **whole spec life-cycle again**:

* BDD: every `beforeEach()`, the `aroundEach()` closures, the spec body and every `afterEach()`.
* xUnit: `setup()`, the test method (with its expected exception checks) and `teardown()`.

The spec passes as soon as one attempt passes. When every attempt fails, the spec reports the failure or error of the **last** attempt. Skipped specs (`skip()`, `xit()`, a `skip` constraint) are never retried. Files [attached](attachments.md) during earlier attempts stay on the spec, so you keep the screenshots of every failed attempt.

## Declaring Retries

### On a spec: the `retries` argument

`it()`, `fit()` and `xit()` take a `retries` argument: how many **extra** times to run the spec. `retries = 2` means up to three attempts.

### On a bundle: the `retries` annotation

A `retries` annotation on the bundle class applies to every spec of the bundle that does not declare its own.

### On an xUnit test: the `retries` method annotation

xUnit test methods take a `retries` annotation, which wins over the bundle annotation.

{% tabs %}
{% tab title="BDD - BoxLang" %}
{% code title="tests/specs/CheckoutSpec.bx" %}
```java
// Every spec of this bundle gets one retry
@retries( 1 )
class extends="testbox.system.BrowserSpec" {

    function run() {
        describe( "Checkout", () => {

            // Uses the bundle retries: up to 2 attempts
            it( "shows the cart", () => {
                browse( ( page ) => {
                    expect( page.visit( "/cart" ) ).toSee( "Your cart" )
                } )
            } )

            // The spec value wins: up to 4 attempts
            it(
                title   = "pays with the sandbox gateway",
                body    = () => {
                    browse( ( page ) => {
                        page.visit( "/checkout" ).click( "Pay now" )
                        expect( page ).toSee( "Thank you" )
                    } )
                },
                retries = 3
            )

        } )
    }

}
```
{% endcode %}
{% endtab %}

{% tab title="xUnit - BoxLang" %}
{% code title="tests/specs/CheckoutTest.bx" %}
```groovy
// Every test of this bundle gets one retry
@retries( 1 )
class extends="testbox.system.BrowserSpec" {

    // Uses the bundle retries: up to 2 attempts
    function testShowsTheCart() {
        browse( ( page ) => {
            expect( page.visit( "/cart" ) ).toSee( "Your cart" )
        } )
    }

    // The method annotation wins: up to 4 attempts
    @retries( 3 )
    function testPaysWithTheSandboxGateway() {
        browse( ( page ) => {
            page.visit( "/checkout" ).click( "Pay now" )
            expect( page ).toSee( "Thank you" )
        } )
    }

}
```
{% endcode %}
{% endtab %}

{% tab title="BDD - CFML" %}
{% code title="tests/specs/PaymentGatewaySpec.cfc" %}
```cfscript
// Every spec of this bundle gets one retry
component extends="testbox.system.BaseSpec" retries="1" {

    function run(){
        describe( "Payment gateway", function(){

            // Uses the bundle retries: up to 2 attempts
            it( "pings the sandbox", function(){
                expect( new models.Gateway().ping() ).toBeTrue();
            } );

            // The spec value wins: up to 4 attempts
            it(
                title   = "charges a card in the sandbox",
                body    = function(){
                    var charge = new models.Gateway().charge( 100 );
                    expect( charge.status ).toBe( "succeeded" );
                },
                retries = 3
            );

        } );
    }

}
```
{% endcode %}
{% endtab %}

{% tab title="xUnit - CFML" %}
{% code title="tests/specs/PaymentGatewayTest.cfc" %}
```cfscript
// Every test of this bundle gets one retry
component extends="testbox.system.BaseSpec" retries="1" {

    // Uses the bundle retries: up to 2 attempts
    function testPingsTheSandbox(){
        $assert.isTrue( new models.Gateway().ping() );
    }

    // The method annotation wins: up to 4 attempts
    function testChargesACardInTheSandbox() retries="3"{
        var charge = new models.Gateway().charge( 100 );
        $assert.isEqual( "succeeded", charge.status );
    }

}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### For the whole run: the `retries` option

Give every spec that declares nothing a default with the `retries` runner option. On the [BoxLang CLI runner](../getting-started/running-tests/boxlang-cli-runner.md):

```bash
./testbox/run --directory=tests.specs --retries=2
```

Or through the `options` of `TestBox` when you run tests from code:

{% tabs %}
{% tab title="BoxLang" %}
```java
var testbox = new testbox.system.TestBox(
    directory = "tests.specs",
    options   = { retries : 2 }
)
println( testbox.run( reporter = "text" ) )
```
{% endtab %}

{% tab title="CFML" %}
```cfscript
var testbox = new testbox.system.TestBox(
    directory = "tests.specs",
    options   = { retries : 2 }
);
writeOutput( testbox.run( reporter = "simple" ) );
```
{% endtab %}
{% endtabs %}

## Precedence

TestBox resolves the retries of a spec in this order, and the first match wins:

| Order | Source | Notes |
| --- | --- | --- |
| 1 | The spec: `it( ..., retries = N )` or the xUnit `retries` method annotation | Only when greater than `0`. `0`, the default, means "inherit". |
| 2 | The bundle `retries` annotation | Any number, so `@retries( 0 )` (or `retries="0"` on a CFML component) turns retries off for the bundle, even when the run sets a global value. |
| 3 | The global `retries` runner option (`--retries=N`) | Applies to specs and bundles that declare nothing. |
| 4 | None | `0`: every spec runs once. |

## Attempts in the Results

Every spec records how many times it ran in the `attempts` key of its stats: `1` for a spec that ran once, more for a retried spec. Reporters call out specs that needed more than one attempt, for example in the text reporter:

```
( √ ) shows the cart (182 ms)
( √ ) pays with the sandbox gateway (4210 ms) (passed after 2 attempts)
( X ) charges a card in the sandbox (9033 ms) (failed after 4 attempts)
```

The text, console and Simple reporters and the BoxLang runner `--stream` output show the note. The JSON and Raw reports carry `attempts` for every spec, so your CI can flag flaky specs:

{% tabs %}
{% tab title="BoxLang" %}
```java
var results = new testbox.system.TestBox( directory = "tests.specs", options = { retries : 2 } ).runRaw()

var flaky = []
var collect = ( suites ) => {
    for ( var suite in suites ) {
        flaky.append( suite.specStats.filter( ( spec ) => spec.status == "Passed" && spec.attempts > 1 ), true )
        collect( suite.suiteStats )
    }
}
for ( var bundle in results.getBundleStats() ) {
    collect( bundle.suiteStats )
}
println( "Flaky specs: " & flaky.map( ( spec ) => spec.name ).toList( ", " ) )
```
{% endtab %}

{% tab title="CFML" %}
```cfscript
var results = new testbox.system.TestBox( directory = "tests.specs", options = { retries : 2 } ).runRaw();

var flaky   = [];
var collect = function( suites ){
    for ( var suite in suites ) {
        arrayAppend( flaky, suite.specStats.filter( function( spec ){
            return spec.status == "Passed" && spec.attempts > 1;
        } ), true );
        collect( suite.suiteStats );
    }
};
for ( var bundle in results.getBundleStats() ) {
    collect( bundle.suiteStats );
}
writeOutput( "Flaky specs: " & flaky.map( function( spec ){ return spec.name; } ).toList( ", " ) );
```
{% endtab %}
{% endtabs %}

## Retries and `--failed`

Retries handle a hiccup inside one run. To rerun only what failed after the run is over, use the BoxLang runner [`--failed`](running-browser-tests.md#rerun-what-failed-failed) option. The two combine well: `./testbox/run --failed --retries=1`.
