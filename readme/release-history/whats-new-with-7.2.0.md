---
description: Coming soon
---

# What's New With 7.2.0

TestBox 7.2.0 adds a reporter designed for AI agents and automation, so a test run can be read with a minimal token cost.

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
