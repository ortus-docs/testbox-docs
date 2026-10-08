---
description: Coming soon
---

# What's New With 7.2.0

TestBox 7.2.0 gives the HTML reporters a complete new look and an **Ask AI** assistant for every failure, and adds a reporter designed for AI agents and automation, so a test run can be read with a minimal token cost.

* * *

## A New Look For The HTML Reporters

The `Simple`, `Min`, `Dot` and `Doc` reporters have been rebuilt on Bootstrap 5.3, Bootstrap Icons and Alpine.js, with light and dark themes on the TestBox palette and the new TestBox logos. They keep every feature they had, and they are still fully inlined, so reports run airgapped. A page also went from about 1.4 MB to about 0.45 MB.

{% embed url="https://youtu.be/FNiiAzqS7tU" %}

<figure><img src="../../.gitbook/assets/reporters-simple-failures-light.png" alt="The Simple reporter in light mode"><figcaption><p>Simple, light mode</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/reporters-simple-failures-dark.png" alt="The Simple reporter in dark mode"><figcaption><p>Simple, dark mode</p></figcaption></figure>

### Highlights

* **The verdict first.** A green or red banner with the totals, a proportion bar and status filters, so a failing run can not be missed. A bundle that could not run gets its own alert above it.
* **Light, dark and system themes**, applied before the page paints and remembered in your browser.
* **Keyboard navigation.** Press `F` to jump to the next failure and `/` to search.
* **Filters everywhere.** Filter the whole report or a single bundle by status, and search by name.
* **Highlighted code** for BoxLang and CFML, with the failing line marked.
* **Run links** on every bundle, suite and spec. They are plain links, so a refresh runs them again.
* **Dot opens a drawer** with the details, the code and Ask AI, instead of an `alert()`. **Doc** has a bundle navigation.
* **Responsive**, so a CI artifact is readable on a phone.

<figure><img src="../../.gitbook/assets/reporters-demo-next-failure.gif" alt="Pressing F to jump between failures"><figcaption><p>Press <code>F</code> to jump from failure to failure</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/reporters-demo-dot-drawer.gif" alt="The Dot reporter opening a drawer"><figcaption><p>Dot opens the details in a drawer</p></figcaption></figure>

### Ask AI

Every failure and error has an **Ask AI** menu. It builds a prompt with the spec, the message, the code around the failing line, the first stack frames and the command that runs only that spec again. You can copy it, preview it, open it in ChatGPT or Claude, or copy the failure as JSON for a coding agent. **Copy all failures for AI** builds one prompt for the whole run.

<figure><img src="../../.gitbook/assets/reporters-demo-ask-ai.gif" alt="Opening the Ask AI menu and previewing the prompt"><figcaption><p>Ask AI menu and prompt preview</p></figcaption></figure>

Ask AI is on by default. Nothing leaves the page until you click a provider, and the first time you do, the page tells you what will happen and waits for your confirmation. Use the `aiAssist`, `aiProviders`, `aiContextLines`, `aiStackFrames` and `aiPrompt` reporter options to configure it, or `?aiAssist=false` to switch it off for a run.

<figure><img src="../../.gitbook/assets/reporters-ai-notice.png" alt="The first-use notice"><figcaption><p>The first-use notice</p></figcaption></figure>

### Reports From Code

The `urlParams` reporter option sets request params such as `editor` and `aiAssist` when you produce a report from code and there is no `url` scope, for example with the BoxLang CLI.

### Upgrade Notes

{% hint style="warning" %}
**Changed in TestBox 7.2:** the `url.fullPage` switch was removed. An HTML reporter always returns a complete page. If you embedded a reporter in your own page, include it in an `<iframe>` or use the `JSON` reporter and render the data yourself.
{% endhint %}

{% hint style="info" %}
The report templates now keep only markup. Their logic lives in `system/reports/ReportHelper.cfc` and `BaseHTMLReporter.cfc`. The reporters share one base class, `BaseHTMLReporter`.
{% endhint %}

Everything about the new reporters is documented in [HTML Reporters](../../digging-deeper/reporters/html-reporters.md).

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
