---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-xunit-primer/reporters
---

# Reporters

TestBox comes also with a nice plethora of reporters:

* **ANTJunit**     : A specific variant of JUnit XML that works with the ANT junitreport task
* **Codexwiki** : Produces MediaWiki syntax for usage in Codex Wiki
* **Console**     : Sends report to console
* **Doc**         : Documentation-style HTML with a bundle navigation, in light and dark
* **Dot**         : One dot per spec, click a dot for its details and Ask AI
* **JSON**         : Builds a report into JSON
* **JUnit**     : Builds a JUnit compliant report
* **Raw**         : Returns the raw structure representation of the testing results
* **Simple**     : The complete HTML report, failures first, with Ask AI and editor links
* **Text**         : Back to the 80's with an awesome text report
* **XML**         : Builds yet another XML testing report
* **Tap**         : A test anything protocol reporter
* **Min**         : A compact HTML view that lists only what needs attention
* **MinText** : A minimalistic view of your test reports for consoles
* **Agent**     : Compact, token-efficient JSON for AI agents and automation
* **NodeJS**    : User-contributed: [https://www.npmjs.com/package/testbox-runner](https://www.npmjs.com/package/testbox-runner)

The HTML reporters (Simple, Min, Dot and Doc) were rebuilt in TestBox 7.2 with light and dark themes, keyboard navigation and **Ask AI** on every failure.

<figure><img src="../../.gitbook/assets/reporters-simple-failures-light.png" alt="The Simple reporter"><figcaption><p>The Simple reporter</p></figcaption></figure>

{% content-ref url="../../digging-deeper/reporters/html-reporters.md" %}
[html-reporters.md](../../digging-deeper/reporters/html-reporters.md)
{% endcontent-ref %}
