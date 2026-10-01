---
description: Drive a real browser from your TestBox specs with bx-playwright.
icon: browsers
---

# Browser Testing

TestBox can drive a real browser (Chromium, Firefox or WebKit) from your specs through the [bx-playwright](https://bxplaywright.boxlang.io) module. Your specs visit pages, click, fill forms and assert on what a user actually sees, with the same `describe()`, `it()` and `expect()` you already write.

{% hint style="info" %}
`BrowserSpec` and the [browser matchers](browser-matchers.md) require BoxLang, because bx-playwright only runs on BoxLang. When the engine is not BoxLang, or bx-playwright is not installed, `browse()` and `this.playwright()` skip the running spec with the reason, so the rest of your suite still runs:

* `Browser specs need the BoxLang engine: bx-playwright only runs on BoxLang`
* `bx-playwright is not installed: install-bx-module bx-playwright`

The features that browser tests rely on, [attachments](attachments.md), [retries](retries.md), `Playwright.AssertionFailed` counted as a failure and the [`--failed` runner option](running-browser-tests.md#rerun-what-failed-failed), work for every spec on every engine.
{% endhint %}

## Why Browser Tests?

Unit and integration specs call your code directly. A browser test exercises the whole stack the way a user does: routing, templates, JavaScript, CSS and the network all take part. bx-playwright brings two things that make those tests reliable:

* **A real browser.** Pages render and run their JavaScript exactly as they do for your users, headless in CI or headed while you debug.
* **Web-first assertions.** Every assertion retries until it passes or its timeout expires (5 seconds by default), so you never sprinkle `sleep()` calls to wait for an element or a redirect.

TestBox adds the testing glue on top: one browser per bundle, fresh isolated pages per spec, `expect()` [matchers](browser-matchers.md) for pages and locators, failure screenshots, traces and videos [attached to the spec](attachments.md), [retries](retries.md) for flaky specs, and runner options to [start your web server and rerun only what failed](running-browser-tests.md).

## Installation

bx-playwright needs BoxLang 1.17+ and Java 21+. Install the module into your BoxLang runtime, then download a browser:

```bash
install-bx-module bx-playwright
bxPlaywright install chromium
bxPlaywright doctor
```

On Linux CI machines add `--with-deps` to `bxPlaywright install` so the system libraries the browser needs are installed too. More browsers, profiles and settings are covered in the [bx-playwright getting started guide](https://bxplaywright.boxlang.io/getting-started/).

## Your First Browser Spec

Extend `testbox.system.BrowserSpec` instead of `testbox.system.BaseSpec`. Inside a spec, call `browse()` with a closure: it receives a fresh page, and everything the page offers (`visit()`, `click()`, `fill()`, finders, locators and assertions) is the [bx-playwright browsing API](https://bxplaywright.boxlang.io/browsing/).

{% tabs %}
{% tab title="BDD - BoxLang" %}
{% code title="tests/specs/LoginSpec.bx" %}
```java
@baseURL( "http://localhost:8080" )
@browserProfile( "ci" )
class extends="testbox.system.BrowserSpec" {

    function run() {
        describe( "Login", () => {

            it( "signs in with valid credentials", () => {
                browse( ( page ) => {
                    page.visit( "/login" )
                        .fill( "Email", "luis@ortus.com" )
                        .fill( "Password", "secret" )
                        .click( "Sign in" )

                    expect( page ).toHavePath( "/dashboard" )
                    expect( page ).toSee( "Welcome back" )
                } )
            } )

            it( "rejects a wrong password", () => {
                browse( ( page ) => {
                    page.visit( "/login" )
                        .fill( "Email", "luis@ortus.com" )
                        .fill( "Password", "nope" )
                        .click( "Sign in" )

                    expect( page ).toHavePath( "/login" )
                    expect( page.locator( "@error" ) ).toHaveText( "Invalid credentials" )
                } )
            } )

        } )
    }

}
```
{% endcode %}
{% endtab %}

{% tab title="xUnit - BoxLang" %}
{% code title="tests/specs/LoginTest.bx" %}
```groovy
@baseURL( "http://localhost:8080" )
@browserProfile( "ci" )
class extends="testbox.system.BrowserSpec" {

    function testSignsInWithValidCredentials() {
        browse( ( page ) => {
            page.visit( "/login" )
                .fill( "Email", "luis@ortus.com" )
                .fill( "Password", "secret" )
                .click( "Sign in" )

            expect( page ).toHavePath( "/dashboard" )
            expect( page ).toSee( "Welcome back" )
        } )
    }

    function testRejectsAWrongPassword() {
        browse( ( page ) => {
            page.visit( "/login" )
                .fill( "Email", "luis@ortus.com" )
                .fill( "Password", "nope" )
                .click( "Sign in" )
                // the bx-playwright inline assertions work too
                .assertPathIs( "/login" )
                .assertSee( "Invalid credentials" )
        } )
    }

}
```
{% endcode %}
{% endtab %}
{% endtabs %}

Run it like any other bundle, for example with the [BoxLang CLI runner](../getting-started/running-tests/boxlang-cli-runner.md):

```bash
./testbox/run --bundles=tests.specs.LoginSpec
```

You can assert in two styles, and mix them freely:

* TestBox [browser matchers](browser-matchers.md): `expect( page ).toSee( "Welcome" )`, `expect( locator ).toBeVisible()`, with `not` forms.
* bx-playwright [assertions](https://bxplaywright.boxlang.io/assertions/): `page.assertSee( "Welcome" )`, `page.expect( "h1" ).toHaveText( "Hi" )`. Their `Playwright.AssertionFailed` exceptions count as spec **failures**, not errors.

## How a `BrowserSpec` Works

* **One browser per bundle.** The bundle starts one bx-playwright manager, and its browser, the first time a spec uses it. It closes after the bundle through the `closeBrowser()` method, which carries the `afterAll` annotation. Your own `beforeAll()` and `afterAll()` need no `super` calls.
* **Fresh pages per `browse()`.** Every `browse()` call gets new pages, each in its own browser context (its own cookies, storage and session), and closes them when the closure ends. Specs never leak state into each other.
* **Matchers registered for you.** The [browser matchers](browser-matchers.md) are registered for every spec of the bundle.
* **Failure artifacts attached.** When the closure throws, the screenshots, trace and videos your artifact policy kept are [attached to the spec](attachments.md#automatic-browser-artifacts) and the exception is rethrown unchanged.

## Bundle Annotations

| Annotation | Description |
| --- | --- |
| `baseURL` | The URL relative visits such as `page.visit( "/login" )` resolve against. |
| `browserProfile` | The bx-playwright [profiles](https://bxplaywright.boxlang.io/profiles/) for the bundle browser, a list such as `ci,mobile`. Profiles merge left to right. |
| `retries` | Rerun failing specs of the bundle. See [Retries](retries.md). |

When `baseURL` is not set, the bundle uses, in order:

1. The `--web-server-url` of the BoxLang runner, when the runner [started your web server](running-browser-tests.md#start-a-web-server-for-the-run) (kept in `server.testbox.webServerURL`).
2. The bx-playwright `baseURL` setting, or the `BX_PLAYWRIGHT_BASEURL` environment variable.

When `browserProfile` is not set, bx-playwright uses its `defaultProfile` setting, or the profile named in the `BX_PLAYWRIGHT_PROFILE` environment variable. That makes `BX_PLAYWRIGHT_PROFILE=ci` a handy switch for CI runs, while `browserProfile` pins a profile for one bundle.

```java
// A mobile, dark mode bundle against staging
@baseURL( "https://staging.example.com" )
@browserProfile( "mobile,dark" )
class extends="testbox.system.BrowserSpec" {
    // ...
}
```

## `browse()`

```java
any function browse( required function callback, struct options = {} )
```

| Argument | Description |
| --- | --- |
| `callback` | The closure to run. It receives one page per declared argument. |
| `options` | Context options for bx-playwright `newContext()`, for example `{ viewport : { width : 390, height : 844 } }`, `{ timeouts : { assertion : 10000 } }` or an `artifacts` policy. |

`browse()` returns whatever the callback returns, so you can pull a value out of the page and assert on it afterwards:

```java
var title = browse( ( page ) => page.visit( "/" ).title() )
expect( title ).toInclude( "Home" )
```

### One page per argument: multi-user flows

Declare one argument per user. Each argument gets its own page in its **own browser context**, so two users can be signed in at once without sharing cookies:

```java
it( "shows a new message to the other user", () => {
    browse( ( alice, bob ) => {
        alice.visit( "/login" ).fill( "Email", "alice@example.com" ).fill( "Password", "secret" ).click( "Sign in" )
        bob.visit( "/login" ).fill( "Email", "bob@example.com" ).fill( "Password", "secret" ).click( "Sign in" )

        alice.visit( "/chat" ).fill( "Message", "Hi Bob" ).click( "Send" )

        // Web-first: waits until the message shows up for Bob
        expect( bob.visit( "/chat" ) ).toSee( "Hi Bob" )
    } )
} )
```

A callback that declares no arguments receives a single page as its first positional argument.

### Context options

The `options` struct applies to every page of the call. For example, a phone sized viewport and a longer assertion timeout:

```java
browse(
    ( page ) => {
        page.visit( "/" )
        expect( page.locator( "@mobile-menu" ) ).toBeVisible()
    },
    { viewport : { width : 390, height : 844 }, timeouts : { assertion : 10000 } }
)
```

See the bx-playwright [configuration reference](https://bxplaywright.boxlang.io/configuration/) for every option.

## `this.playwright()`

`this.playwright()` returns the bundle's bx-playwright manager, created on first use with the `browserProfile` and `baseURL` annotations. Use it for anything beyond `browse()`, such as the API request client, saved sessions or a page you manage yourself. Everything it starts closes after the bundle.

```java
it( "lists the products from the API", () => {
    var response = this.playwright().request().get( "/api/products" )
    expect( response.status() ).toBe( 200 )
} )
```

{% hint style="warning" %}
Call it as `this.playwright()`. An unqualified `playwright()` resolves to the bx-playwright built-in function, even inside a `BrowserSpec`, and returns a **new** manager that ignores the bundle annotations and that the bundle does not close for you.
{% endhint %}

## Skipping When No Browser Is Available

`browse()` and `this.playwright()` already skip the running spec when bx-playwright is missing. To skip whole suites up front, without even entering them, use `browserAvailable()` in a `skip` constraint:

```java
function run() {
    describe( title = "Checkout", skip = !browserAvailable(), body = () => {
        it( "pays with a card", () => {
            browse( ( page ) => {
                // ...
            } )
        } )
    } )
}
```

## Using the Browser From Any Spec

`BrowserSpec` delegates to `testbox.system.browser.BrowserSupport`, which any BoxLang spec can use directly. Close it yourself when you are done:

```java
class extends="testbox.system.BaseSpec" {

    function beforeAll() {
        variables.browser = new testbox.system.browser.BrowserSupport( this )
        addMatchers( new testbox.system.browser.BrowserMatchers() )
    }

    function afterAll() {
        variables.browser.close()
    }

    function run() {
        describe( "Home page", () => {
            it( "greets visitors", () => {
                variables.browser.browse( ( page ) => {
                    page.visit( "http://localhost:8080/" )
                    expect( page ).toSee( "Welcome" )
                } )
            } )
        } )
    }

}
```

## Thread Safety

A bx-playwright manager is not thread safe, and a bundle shares one. Do not use `asyncAll` on suites, or bundles, that browse. Run browser bundles sequentially.

## Learn More

{% content-ref url="browser-matchers.md" %}
[browser-matchers.md](browser-matchers.md)
{% endcontent-ref %}

{% content-ref url="attachments.md" %}
[attachments.md](attachments.md)
{% endcontent-ref %}

{% content-ref url="retries.md" %}
[retries.md](retries.md)
{% endcontent-ref %}

{% content-ref url="running-browser-tests.md" %}
[running-browser-tests.md](running-browser-tests.md)
{% endcontent-ref %}

The browser API itself is documented in the bx-playwright guides: [Browsing](https://bxplaywright.boxlang.io/browsing/), [Assertions](https://bxplaywright.boxlang.io/assertions/), [Testing](https://bxplaywright.boxlang.io/testing/) (artifacts, debugging, page objects) and [Building on bx-playwright](https://bxplaywright.boxlang.io/integrations/).
