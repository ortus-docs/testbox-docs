---
description: Return compact TestBox results for AI agents and automation.
icon: robot
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/running-tests/ai-agent-reporter
---

# AI Agent Reporter

The `AgentReporter` returns a compact, minified JSON summary designed for AI agents and automation. It includes run totals and failed or errored specs, without the full result details produced by the standard JSON reporter.

This is a **reporter**, not a separate test runner. Use it with a runner that supports the `agent` reporter:

{% tabs %}
{% tab title="BoxLang" %}
{% code title="BoxLang CLI runner" %}

```bash
./testbox/run --reporter=agent
```

{% endcode %}
{% endtab %}

{% tab title="CFML" %}
{% code title="CFML runner URL" %}

```text
runner.cfm?reporter=agent&directory=tests.specs
```

{% endcode %}
{% endtab %}
{% endtabs %}

The JSON includes an `ok` status, run totals, a `failures` array, and a `truncated` count. See [AgentReporter options and output](../../digging-deeper/reporters/README.md) for the complete schema and configuration options.
