---
description: Coming soon
---

# What's New With 7.2.0

TestBox 7.2.0 brings **browser testing** to TestBox. Built on the [bx-playwright](https://bxplaywright.boxlang.io) module, BoxLang specs can now drive a real browser, assert on pages with web-first matchers, and keep screenshots, traces and videos of every failure. Around it, TestBox gains features that every test suite benefits from, on every engine: spec attachments, retries, and BoxLang runner options to start your web server and rerun only what failed. It also adds the **Agent reporter**, built for AI agents and automation, so a test run can be read with a minimal token cost.

{% hint style="info" %}
This release is not out yet. The release date will be added here when it ships.
{% endhint %}

* * *

## Browser Testing With `BrowserSpec`

{% hint style="info" %}
`BrowserSpec` and the browser matchers require BoxLang and the bx-playwright module. On a CFML engine, or without bx-playwright, `browse()` skips the running spec with the reason, such as `bx-playwright is not installed: install-bx-module bx-playwright`.
{% endhint %}

Extend `testbox.system.BrowserSpec` and call `browse()`: every declared closure argument gets a fresh page in its own isolated browser context, and the bundle shares one browser that closes after the bundle.

```java
@baseURL( "http://localhost:8080" )
@browserProfile( "ci" )
class extends="testbox.system.BrowserSpec" {

    function run() {
        describe( "Login", () => {
            it( "signs in", () => {
                browse( ( page ) => {
                    page.visit( "/login" )
                        .fill( "Email", "luis@ortus.com" )
                        .fill( "Password", "secret" )
                        .click( "Sign in" )
                    expect( page ).toHavePath( "/dashboard" )
                    expect( page ).toSee( "Welcome" )
                } )
            } )
        } )
    }

}
```

* `browserProfile` and `baseURL` class annotations.
* `browse( callback, options )` with one page per declared argument, for multi-user flows.
* `this.playwright()` for the bundle's bx-playwright manager, and `browserAvailable()` for skip constraints.
* The logic lives in `testbox.system.browser.BrowserSupport`, usable from any BoxLang spec.

Read the [Browser Testing guide](../../browser-testing/README.md).

## Browser Matchers

Nine web-first matchers for bx-playwright pages and locators, each with its `not` form. They retry until they pass or the assertion timeout expires, and fail with Playwright's expected, received and call log message.

```java
expect( page ).toHaveTitle( "Dashboard" )
expect( page ).toHaveURL( page.regex( "/orders/\d+$" ) )
expect( page ).toHavePath( "/dashboard" )
expect( page ).toSee( "Welcome" )
expect( page.locator( "h1" ) ).toHaveText( "Your Orders" )
expect( page.locator( "@menu" ) ).toBeVisible()
expect( page.locator( ".spinner" ) ).toBeHidden()
expect( page.locator( ".cart li" ) ).toHaveCount( 2 )
expect( page.byLabel( "Email" ) ).toHaveValue( "luis@ortus.com" )
```

`BrowserSpec` registers them for you; any other BoxLang spec can use `addMatchers( new testbox.system.browser.BrowserMatchers() )`. See [Browser Matchers](../../browser-testing/browser-matchers.md).

## Spec Attachments: `attach()`

Every spec can attach files to its result, on every engine: `attach( path, type = "file", name = "" )`. A failed `browse()` attaches the screenshots, trace and videos bx-playwright kept.

{% tabs %}
{% tab title="BoxLang" %}
```java
it( "renders the PDF", () => {
    var pdf = getTempDirectory() & "report.pdf"
    new models.Reports().monthly( pdf )
    attach( pdf, "file", "Monthly report" )
} )
```
{% endtab %}

{% tab title="CFML" %}
```cfscript
it( "renders the PDF", function(){
    var pdf = getTempDirectory() & "report.pdf";
    new models.Reports().monthly( pdf );
    attach( pdf, "file", "Monthly report" );
} );
```
{% endtab %}
{% endtabs %}

Attachments land in the new `attachments` array of the spec stats. The JSON report includes them, the Simple report links them, the text, console and stream outputs list them under failed specs, and the JUnit and ANT JUnit reports add one `[[ATTACHMENT|path]]` line per file in `<system-out>`, the format the Jenkins JUnit Attachments plugin and GitLab understand. See [Attachments](../../browser-testing/attachments.md).

## Spec Retries

Rerun a failing or erroring spec, with its `beforeEach()` and `afterEach()` (or `setup()` and `teardown()`), up to N more times, on every engine:

{% tabs %}
{% tab title="BoxLang" %}
```java
@retries( 1 )
class extends="testbox.system.BaseSpec" {

    function run() {
        describe( "Payment gateway", () => {
            it( title = "charges a card", body = () => {
                expect( gateway.charge( 100 ).status ).toBe( "succeeded" )
            }, retries = 3 )
        } )
    }

}
```
{% endtab %}

{% tab title="CFML" %}
```cfscript
component extends="testbox.system.BaseSpec" retries="1" {

    function run(){
        describe( "Payment gateway", function(){
            it( title = "charges a card", body = function(){
                expect( gateway.charge( 100 ).status ).toBe( "succeeded" );
            }, retries = 3 );
        } );
    }

}
```
{% endtab %}
{% endtabs %}

* A `retries` argument on `it()`, `fit()` and `xit()`, a `retries` bundle annotation, a `retries` method annotation for xUnit tests, and a global `retries` runner option (`--retries=N` on the BoxLang runner).
* The spec value wins over the bundle annotation, which wins over the global option. Skipped specs are never retried.
* The new `attempts` spec stat counts the runs, and reporters show `(passed after 2 attempts)`.

See [Retries](../../browser-testing/retries.md).

## BoxLang Runner: `--failed`

Every run writes `{reportpath}/.testbox-failed.json` with the bundles and spec ids that failed or errored. `./testbox/run --failed` reruns only those.

## BoxLang Runner: `--web-server`

Start your application before the tests, wait until it answers, and stop it with its child processes afterwards:

```bash
./testbox/run --web-server="boxlang-miniserver --port 8080" --web-server-url=http://localhost:8080 --web-server-timeout=60
```

The runner exits with code 1 when the server does not answer in time. The URL becomes the default `baseURL` of `BrowserSpec` bundles. See [Running Browser Tests](../../browser-testing/running-browser-tests.md).

* * *

## Agent Reporter

The new `Agent` reporter returns a single minified JSON line with the totals and only the specs that failed or errored. A passing run is a few dozen tokens, where the `JSON` reporter returns the full result set.

{% tabs %}
{% tab title="BoxLang" %}
{% code title="BoxLang CLI runner" %}
```bash
./testbox/run --reporter=agent
```
{% endcode %}

{% code title="Programmatic" %}
```java
var testbox = new testbox.system.TestBox(
    bundles  = "tests.specs",
    reporter = {
        type    : "testbox.system.reports.AgentReporter",
        options : { maxFailures : 10 }
    }
)
println( testbox.run() )
```
{% endcode %}
{% endtab %}

{% tab title="CFML" %}
{% code title="Programmatic" %}
```cfscript
var testbox = new testbox.system.TestBox(
    bundles  = "tests.specs",
    reporter = {
        type    : "testbox.system.reports.AgentReporter",
        options : { maxFailures : 10 }
    }
);
writeOutput( testbox.run() );
```
{% endcode %}
{% endtab %}
{% endtabs %}

```json
{"ok":false,"totals":{"pass":120,"fail":2,"error":1,"skipped":3,"specs":126,"ms":4210},"failures":[{"bundle":"tests.specs.FooTest","spec":"Foo > can add","status":"failed","message":"Expected [4] but received [3]","at":"tests/specs/FooTest.cfc:42"}],"truncated":0}
```

You can control the amount of detail with the `detail`, `maxFailures`, `maxMessageLength`, `includeStack`, `stackDepth`, `includeSkipped` and `includeDebug` options. See the full reference in [Reporters](../../digging-deeper/reporters/README.md#agentreporter---token-efficient-output-for-ai-agents).

## Changes

### Playwright Assertion Failures Are Failures

{% hint style="warning" %}
**Changed in TestBox 7.2:** a `Playwright.AssertionFailed` exception now counts as a spec **failure**, like `TestBox.AssertionFailed`, in BDD specs and xUnit tests, keeping its message and detail. It used to count as an error. Other `Playwright.*` exceptions, such as `Playwright.Timeout`, still count as errors.
{% endhint %}

### xUnit Failure Detail

xUnit failures now also record the failure detail in the spec stats, as BDD failures already did.

## Fixes

### `run` Script Quoting

The `run` launcher script now quotes its arguments, so runner options with spaces, such as a `--web-server` command, reach the BoxLang runner intact.

* * *
