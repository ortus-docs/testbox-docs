---
description: TestBox expectations for bx-playwright pages and locators that wait for the page.
icon: list-check
---

# Browser Matchers

The browser matchers let you assert on bx-playwright pages and locators with the TestBox `expect()` DSL you already use:

```java
expect( page ).toHaveTitle( "Dashboard" )
expect( page ).toHavePath( "/dashboard" )
expect( page ).toSee( "Welcome" )
expect( page ).notToSee( "Error" )
expect( page.locator( ".todo" ) ).toHaveCount( 3 )
expect( page.locator( "@error" ) ).notToBeVisible()
```

{% hint style="info" %}
These matchers require BoxLang: they live in `testbox.system.browser.BrowserMatchers`, a BoxLang class built on bx-playwright. [Browser](README.md#turning-on-browser-support) bundles skip their specs on CFML engines, or when bx-playwright is not installed, before any matcher runs.
{% endhint %}

## Web-First: They Wait

Every browser matcher delegates to bx-playwright's [web-first assertions](https://bxplaywright.boxlang.io/assertions/). It **retries until it passes or the assertion timeout expires** (5 seconds by default), so a matcher waits for a redirect to land, a spinner to go away or a list to fill in:

```java
browse( ( page ) => {
    page.visit( "/todos" ).fill( "New todo", "Write docs" ).press( "New todo", "Enter" )
    // No sleep: waits until the third item renders
    expect( page.locator( ".todos li" ) ).toHaveCount( 3 )
} )
```

The `not` forms wait too: `notToSee( "Loading" )` passes as soon as the text goes away, instead of failing because it was still there on the first check.

Change the timeout with the bx-playwright `timeouts.assertion` setting, or per `browse()` call:

```java
browse( ( page ) => {
    // ...
}, { timeouts : { assertion : 10000 } } )
```

## Matchers at a Glance

| Matcher | Target | Passes when |
| --- | --- | --- |
| [`toHaveTitle( title )`](#tohavetitle) | page | The page title is exactly `title`, or matches a regex |
| [`toHaveURL( url )`](#tohaveurl) | page | The full page URL is exactly `url`, or matches a regex |
| [`toHavePath( path )`](#tohavepath) | page | The URL path is exactly `path`, ignoring scheme, host, query string and hash |
| [`toSee( text )`](#tosee) | page or locator | `text`, or a regex, is visible in the page or inside the locator |
| [`toHaveText( text )`](#tohavetext) | locator | The text of the first match is exactly `text` (whitespace normalized), or matches a regex |
| [`toBeVisible()`](#tobevisible) | locator | The first match is visible |
| [`toBeHidden()`](#tobehidden) | locator | The first match is hidden or not in the page |
| [`toHaveCount( count )`](#tohavecount) | locator | The locator matches exactly `count` elements |
| [`toHaveValue( value )`](#tohavevalue) | locator | The input, textarea or select has `value`, or a value matching a regex |

Every matcher has a negated form with the `not` prefix: `notToHaveTitle()`, `notToHaveURL()`, `notToHavePath()`, `notToSee()`, `notToHaveText()`, `notToBeVisible()`, `notToBeHidden()`, `notToHaveCount()` and `notToHaveValue()`. See the [Not Operator](../digging-deeper/expectations/not-operator.md).

### Pages and Locators

* A **page** is what `browse()` hands your closure, or what `visit()` and `newPage()` return.
* A **locator** points at elements of a page: `page.locator( ".todo" )`, `page.locator( "@error" )` (a `data-testid`), `page.byRole( "button", { name : "Save" } )`, `page.byLabel( "Email" )` and the other bx-playwright [finders](https://bxplaywright.boxlang.io/browsing/).

Arguments can be passed by position or by name (`toHaveCount( count = 3 )`). Where the table says "or a regex", pass a pattern built with `page.regex()`:

```java
expect( page ).toHaveTitle( page.regex( "^Order ##\d+" ) )
expect( page ).toHaveURL( page.regex( "/orders/\d+$" ) )
```

## Page Matchers

### `toHaveTitle()`

```java
expect( page ).toHaveTitle( required title )
expect( page ).notToHaveTitle( required title )
```

The page title is exactly `title`, or matches a `page.regex()` pattern.

```java
browse( ( page ) => {
    page.visit( "/dashboard" )
    expect( page ).toHaveTitle( "Dashboard" )
    expect( page ).toHaveTitle( page.regex( "^Dash" ) )
    expect( page ).notToHaveTitle( "Login" )
} )
```

### `toHaveURL()`

```java
expect( page ).toHaveURL( required url )
expect( page ).notToHaveURL( required url )
```

The full page URL, scheme, host, path and query string included, is exactly `url`, or matches a `page.regex()` pattern. Use [`toHavePath()`](#tohavepath) when only the path matters.

```java
expect( page ).toHaveURL( "http://localhost:8080/search?q=boxlang" )
expect( page ).toHaveURL( page.regex( "q=boxlang" ) )
expect( page ).notToHaveURL( page.regex( "/login" ) )
```

### `toHavePath()`

```java
expect( page ).toHavePath( required path )
expect( page ).notToHavePath( required path )
```

The URL path is exactly `path` (case sensitive), ignoring the scheme, host, query string and hash, like bx-playwright's `assertPathIs()`. It is the natural check after a redirect, because it does not depend on the `baseURL`:

```java
page.visit( "/login" )
    .fill( "Email", "luis@ortus.com" )
    .fill( "Password", "secret" )
    .click( "Sign in" )
expect( page ).toHavePath( "/dashboard" )      // http://localhost:8080/dashboard?tab=1 passes
expect( page ).notToHavePath( "/login" )
```

{% hint style="info" %}
`toHavePath()` is also a [Data Navigator matcher](../digging-deeper/expectations/data-navigator.md). The browser version only takes over for bx-playwright pages: for any other value, such as a struct, it runs the Data Navigator `toHavePath()`, so both keep working in the same spec.
{% endhint %}

### `toSee()`

```java
expect( pageOrLocator ).toSee( required text )
expect( pageOrLocator ).notToSee( required text )
```

`text`, or a `page.regex()` pattern, is visible in the page, or inside the locator when you pass one. It uses bx-playwright's `assertSee()`, and the negated form uses `assertDontSee()`.

```java
expect( page ).toSee( "Welcome back, Luis" )
expect( page ).notToSee( "Something went wrong" )

// Scope the search to part of the page
expect( page.locator( "nav" ) ).toSee( "Sign out" )
expect( page.locator( "nav" ) ).notToSee( "Sign in" )
```

## Locator Matchers

### `toHaveText()`

```java
expect( locator ).toHaveText( required text )
expect( locator ).notToHaveText( required text )
```

The text of the first matching element is exactly `text`, with whitespace normalized, or matches a `page.regex()` pattern. Use [`toSee()`](#tosee) on the locator for a partial match.

```java
expect( page.locator( "h1" ) ).toHaveText( "Your Orders" )
expect( page.locator( "@total" ) ).toHaveText( page.regex( "^\$\d+\.\d{2}$" ) )
expect( page.locator( "@status" ) ).notToHaveText( "Pending" )
```

### `toBeVisible()`

```java
expect( locator ).toBeVisible()
expect( locator ).notToBeVisible()
```

The first matching element is visible.

```java
page.click( "Menu" )
expect( page.locator( "@menu" ) ).toBeVisible()
expect( page.locator( "@error" ) ).notToBeVisible()
```

### `toBeHidden()`

```java
expect( locator ).toBeHidden()
expect( locator ).notToBeHidden()
```

The first matching element is hidden, or not in the page at all. Handy to wait for spinners and dialogs to go away:

```java
page.click( "Save" )
expect( page.locator( ".spinner" ) ).toBeHidden()
expect( page.locator( "@toast" ) ).notToBeHidden()
```

### `toHaveCount()`

```java
expect( locator ).toHaveCount( required count )
expect( locator ).notToHaveCount( required count )
```

The locator matches exactly `count` elements.

```java
expect( page.locator( ".cart li" ) ).toHaveCount( 2 )
expect( page.locator( ".cart li" ) ).notToHaveCount( 0 )
```

### `toHaveValue()`

```java
expect( locator ).toHaveValue( required value )
expect( locator ).notToHaveValue( required value )
```

The input, textarea or select has `value`, or a value matching a `page.regex()` pattern.

```java
page.fill( "Email", "luis@ortus.com" )
expect( page.byLabel( "Email" ) ).toHaveValue( "luis@ortus.com" )
expect( page.locator( "##country" ) ).toHaveValue( "US" )
expect( page.locator( "input[name=zip]" ) ).notToHaveValue( "" )
```

{% hint style="success" %}
Remember to escape `#` as `##` inside BoxLang strings, as in `page.locator( "##country" )`.
{% endhint %}

## Failure Messages

A matcher that does not pass in time fails the spec with a regular `TestBox.AssertionFailed` failure that carries bx-playwright's message: what was expected, what was received, and Playwright's call log of what it waited for. For example, `expect( page.locator( "li" ) ).toHaveCount( 5 )` on a list with one item fails with a message like:

```
Locator expected to have count: 5
Received: 1
Call log:
  - Expect "toHaveCount" with timeout 5000ms
  - waiting for locator("li")
  ...
```

The exact wording comes from Playwright. `withContext()` [prefixes](../digging-deeper/expectations/README.md#adding-context-to-failures) it like any other matcher.

When the actual value is not the kind of object a matcher needs, such as a locator passed to a page matcher or a plain string, it fails right away, without waiting, and says what it got:

```
toSee() needs a bx-playwright page or locator, but the actual value is the simple value [plain text].
toHaveTitle() needs a bx-playwright page, but the actual value is a struct.
```

## Using the Matchers Outside Browser Specs

TestBox registers the matchers for every spec of a bundle with [browser support](README.md#turning-on-browser-support), turned on by `@browser`, `@browserProfile` or `@baseURL`. In any other BoxLang spec, register them yourself with [`addMatchers()`](../digging-deeper/expectations/custom-matchers.md):

```java
class extends="testbox.system.BaseSpec" {

    function beforeAll() {
        addMatchers( new testbox.system.browser.BrowserMatchers() )
    }

    function run() {
        describe( "Home page", () => {
            it( "greets visitors", () => {
                playwright().browse( ( page ) => {
                    page.visit( "http://localhost:8080/" )
                    expect( page ).toSee( "Welcome" )
                    expect( page.locator( "h1" ) ).toHaveText( "Hello" )
                } )
            } )
        } )
    }

}
```

## Beyond These Matchers

bx-playwright ships many more web-first checks: enabled, checked, focused, attributes, CSS classes, accessible names and more. Use them through `page.expect( selector )` or the inline `assert*()` methods; their `Playwright.AssertionFailed` exceptions count as TestBox failures:

```java
page.expect( "@save" ).toBeEnabled()
page.expect( "@terms" ).not().toBeChecked()
page.assertAttribute( "@avatar", "alt", "Luis" )
```

See the full list in the [bx-playwright assertions guide](https://bxplaywright.boxlang.io/assertions/).
