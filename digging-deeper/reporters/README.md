---
icon: file-chart-pie
metaLinks:
  alternates:
    - https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/digging-deeper/reporters
---

# Reporters

TestBox ships with a rich set of reporters for every use case:

| Reporter | Description |
|----------|-------------|
| `Agent` | Compact, token-efficient JSON for AI agents and automation |
| `ANTJunit` | JUnit XML variant compatible with the ANT `junitreport` task |
| `Codexwiki` | MediaWiki syntax for use in Codex Wiki (DEPRECATED) |
| `Console` | Sends the report to the console |
| `Doc` | Semantic HTML for documentation-style output |
| `Dot` | Compact dot-matrix report (DEPRECATED) |
| `JSON` | Full JSON report of all results |
| `JUnit` | Standard JUnit-compliant XML report |
| `Min` | Minimalistic HTML view |
| `MinText` | Minimalistic plain-text report |
| `Raw` | Raw BoxLang/CFML struct representation of results |
| `Simple` | Basic HTML reporter with editor link support |
| `Tap` | Test Anything Protocol (TAP) output (DEPRECATED) |
| `Text` | Full plain-text report |
| `XML` | XML-based testing report |

To use a specific reporter, append `reporter` to your runner URL, e.g. `&reporter=Text`, or set it in your `runner.bxm` / `runner.cfm`.

## `ConsoleReporter` — Hiding Skipped Tests

The `ConsoleReporter` now accepts a `hideSkipped` option (default `false`) that suppresses skipped spec output — useful when you have many pending specs and want cleaner terminal output.

```javascript
var testbox = new testbox.system.TestBox(
    bundles  = "tests.specs",
    reporter = {
        type    : "testbox.system.reports.ConsoleReporter",
        options : { hideSkipped : true }
    }
);
```

When using the BoxLang CLI runner, pass `--show-skipped=false` instead:

```bash
./testbox/run --show-skipped=false
```

## `AgentReporter` - Token-Efficient Output for AI Agents

**Added in TestBox 7.2.** The `Agent` reporter is a compact JSON reporter for AI agents and automation. It reports the totals plus only the specs that failed or errored, as a single minified line. A passing run costs a few dozen tokens, where the `JSON` reporter returns the full result set.

{% tabs %}
{% tab title="BoxLang" %}
{% code title="Programmatic" %}
```java
var testbox = new testbox.system.TestBox(
    bundles  = "tests.specs",
    reporter = {
        type    : "testbox.system.reports.AgentReporter",
        options : { maxFailures : 10, includeStack : true }
    }
)
println( testbox.run() )
```
{% endcode %}

{% code title="BoxLang CLI runner" %}
```bash
./testbox/run --reporter=agent
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
        options : { maxFailures : 10, includeStack : true }
    }
);
writeOutput( testbox.run() );
```
{% endcode %}

{% code title="HTML runner URL" %}
```
runner.cfm?reporter=agent&directory=tests.specs
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Output

```json
{"ok":false,"totals":{"pass":120,"fail":2,"error":1,"skipped":3,"specs":126,"ms":4210},"failures":[{"bundle":"tests.specs.FooTest","spec":"Foo > can add","status":"failed","message":"Expected [4] but received [3]","at":"tests/specs/FooTest.cfc:42"}],"truncated":0}
```

| Key | Description |
|-----|-------------|
| `ok` | `true` when nothing failed or errored. Agents can branch on this alone. |
| `totals` | `pass`, `fail`, `error`, `skipped`, `specs` and `ms` for the whole run. |
| `failures` | One entry per failed or errored spec: `bundle`, `spec` (suite path joined with ` > `), `status` (`failed` or `error`), `message` and `at` (`file:line`, relative to the web or working root). |
| `truncated` | How many failures were left out because of `maxFailures`. |

Bundle-level exceptions, such as a failing `beforeAll()`, appear in `failures` with an empty `spec` and the `error` status.

### Options

| Option | Default | Description |
|--------|---------|-------------|
| `detail` | `failures` | `summary` returns totals only, `failures` adds the failures, `all` also adds a compact `specs` list (`bundle`, `spec`, `status`, `ms`). |
| `maxFailures` | `20` | Maximum failures listed. `0` means unlimited. The overflow is reported in `truncated`. |
| `maxMessageLength` | `300` | Truncates each failure message. `0` means unlimited. |
| `includeStack` | `false` | Adds a `stack` array of `file:line` frames to each failure, user code first. |
| `stackDepth` | `3` | Number of frames kept when `includeStack` is `true`. |
| `includeSkipped` | `false` | Adds a `skipped` array of spec paths. |
| `includeDebug` | `false` | Adds a `debug` array with the `debug()` output of the run. |

{% hint style="info" %}
The reporter does not change the process exit code. Read the `ok` key to decide whether the run passed.
{% endhint %}

## `StreamingReporter` — Real-Time SSE Output 🆕

The new `StreamingReporter` (backed by `StreamingRunner`) pushes each spec result to the client in real time via Server-Sent Events. It powers both the [TestBox RUN IDE](../../getting-started/running-tests/testbox-run-ide.md) and the `testbox run --streaming` command.

{% content-ref url="../../getting-started/running-tests/streaming-runner.md" %}
[streaming-runner.md](../../getting-started/running-tests/streaming-runner.md)
{% endcontent-ref %}

## Attachments and Retries

Specs can [attach files](../../browser-testing/attachments.md), such as screenshots, traces or logs, and can be [retried](../../browser-testing/retries.md). Reporters show both:

| Reporter | Attachments | Retries |
|----------|-------------|---------|
| `JSON`, `Raw` | An `attachments` array of `{ path, type, name }` in every spec's stats | An `attempts` count in every spec's stats |
| `Simple` | A linked list under each spec | `(passed after N attempts)` after the duration |
| `JUnit`, `ANTJunit` | A `<system-out>` with one `[[ATTACHMENT\|path]]` line per file, read by the Jenkins JUnit Attachments plugin and GitLab | |
| `Text`, `Console` | Listed under failed and errored specs | `(passed after N attempts)` after the duration |

See [Attachments in Reports](../../browser-testing/attachments.md#attachments-in-reports) for an example of the JUnit output.

## Run All and Run Failed

The `Simple`, `Min`, `Dot` and `Doc` reports have a **Run All** button. When something failed or errored, a **Run Failed (N)** button sits next to it and reruns only those specs.

![The Simple report with Run All and Run Failed](../../.gitbook/assets/testbox-sc-run-failed.png)

* The link is built from the report on screen, `?testBundles=...&testSpecs=...` with the failed bundles and spec ids, so the web runner keeps no state between runs. Fix, click it again, repeat.
* When the spec ids would make the link longer than 2000 characters, it reruns the failed bundles whole.
* Bundles that failed outside of a spec, for example in `beforeAll()`, are left out: they are broken, not failed. The report shows them as bundle exceptions.
* The same list is available from code on every engine with `TestResult.getFailedTargets()`, and the BoxLang CLI runner keeps it between runs for [`--failed`](../../browser-testing/running-browser-tests.md#rerun-what-failed-failed).

{% tabs %}
{% tab title="BoxLang" %}
```java
var results = new testbox.system.TestBox( directory = "tests.specs" ).runRaw()
var failed  = results.getFailedTargets()
// { bundles : [ "tests.specs.LoginSpec" ], specs : [ "spec id" ], bundleErrors : [] }

if ( failed.bundles.len() ) {
	new testbox.system.TestBox( bundles = failed.bundles )
		.runRaw( testBundles = failed.bundles, testSpecs = failed.specs )
}
```
{% endtab %}

{% tab title="CFML" %}
```cfscript
var results = new testbox.system.TestBox( directory = "tests.specs" ).runRaw();
var failed  = results.getFailedTargets();

if ( arrayLen( failed.bundles ) ) {
	new testbox.system.TestBox( bundles = failed.bundles )
		.runRaw( testBundles = failed.bundles, testSpecs = failed.specs );
}
```
{% endtab %}
{% endtabs %}

## Open In Editor (Simple Reporter)

The `simple` reporter allows you to set a code editor of choice so it creates clickable links for stack traces and tag contexts — opening exceptions in your editor at the exact line.

{% hint style="info" %}
The default editor is `vscode`.
{% endhint %}

Use the `url.editor` parameter in the URL or set it in your `runner.cfm`:

```markup
<cfsetting showDebugOutput="false">
<!--- Executes all tests in the 'specs' folder with simple reporter by default --->
<cfparam name="url.reporter" 			default="simple">
<cfparam name="url.directory" 			default="tests.specs">
<cfparam name="url.recurse" 			default="true" type="boolean">
<cfparam name="url.bundles" 			default="">
<cfparam name="url.labels" 				default="">
<cfparam name="url.excludes" 			default="">
<cfparam name="url.reportpath" 			default="#expandPath( "/tests/results" )#">
<cfparam name="url.propertiesFilename" 	default="TEST.properties">
<cfparam name="url.propertiesSummary" 	default="false" type="boolean">
<cfparam name="url.editor" 				default="vscode">

<!--- Include the TestBox HTML Runner --->
<cfinclude template="/testbox/system/runners/HTMLRunner.cfm" >
```

![](<../../.gitbook/assets/screen-shot-2021-05-24-at-5.25.20-pm (2) (1).png>)

![](<../../.gitbook/assets/Screen Shot 2021-05-24 at 5.25.29 PM.png>)

### Available Editors

* atom
* emacs
* espresso
* idea
* macvim
* sublime
* textmate
* vscode
* vscode-insiders

## Reporter Screenshots

![](../../.gitbook/assets/testbox-sc-dots.png)

![](../../.gitbook/assets/testbox-sc-json.png)

![](../../.gitbook/assets/testbox-sc-junit.png)

![](../../.gitbook/assets/testbox-sc-simple.png)

![](../../.gitbook/assets/testbox-sc-text.png)

![](../../.gitbook/assets/testbox-sc-xml.png)
