---
description: Get up and running quickly
icon: sign-posts-wrench
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/installing-testbox
---

# Installation

## Framework

TestBox can be installed via [CommandBox CLI](https://www.ortussolutions.com/products/commandbox) as a development dependency in the root of your projects:

```bash
// Create a new project
mkdir myProject --cd

// latest stable version
box install testbox --saveDev

// latest bleeding edge
box install testbox@be --saveDev
```

{% hint style="info" %}
Please note the `--saveDev` flag, which tells CommandBox that TestBox is a development dependency and not a production dependency.
{% endhint %}

{% hint style="danger" %}
DO NOT USE TESTBOX IN PRODUCTION.
{% endhint %}

The only requirement is that it be in either in the webroot or in a location where you create a `/testbox` mapping to it's folder.

### System Requirements


| Engine | Support |
|--------|---------|
| <img src="../../.gitbook/assets/image (8).png" alt="" data-size="line"> **BoxLang 1.x+** | ✅ **Preferred** |
| Lucee 6.x | ✅ Supported |
| Lucee 7.x | ✅ Supported (new in 7.0) |
| Adobe ColdFusion 2023 | ✅ Supported (deprecated) |
| Adobe ColdFusion 2025 | ✅ Supported |
| Lucee 5.x | ⚠️ Deprecated — upgrade recommended |
| Adobe ColdFusion 2021 | ❌ Dropped as of TestBox 7 |

{% hint style="success" %}
TestBox is designed first and foremost for [BoxLang](https://boxlang.io) and remains fully compatible with modern CFML engines.
{% endhint %}

{% hint style="warning" %}
Adobe ColdFusion 2021 support has been **dropped** as of TestBox 7. Please upgrade to Adobe 2023+ or migrate to [BoxLang](https://boxlang.io).
{% endhint %}

### What's Included

Here is a table of what's included in the installation package which goes in `/{project}/testbox`

<table><thead><tr><th width="206">Folder</th><th>Description</th></tr></thead><tbody><tr><td><strong>bx</strong></td><td>BoxLang tools</td></tr><tr><td><strong>cfml</strong></td><td>CFML Tools</td></tr><tr><td><strong>system</strong></td><td>The main system framework folder</td></tr><tr><td><strong>test-visualizer</strong></td><td>A static visualizer of JSON reports. Just drop in a <code>test-results.json</code> and run it!</td></tr><tr><td><strong>tests</strong></td><td>Several sample tests and runners are actually used to build TestBox</td></tr></tbody></table>

#### <img src="../../.gitbook/assets/image (6).png" alt="" data-size="line"> BoxLang Tools

In the `bx` folder you will find specific BoxLang tools:

<table><thead><tr><th width="166">Folder</th><th>Description</th></tr></thead><tbody><tr><td><strong>browser</strong></td><td>This is a little utility to facilitate navigating big testing suites. This helps navigate to the suites you want and execute them instead of typing all the time.</td></tr><tr><td><strong>tests</strong></td><td>A vanilla test runner for any application</td></tr><tr><td><strong>runner</strong></td><td>A simple GUI test runner</td></tr></tbody></table>

#### CFML Tools

In the `cfml` folder you will find specific BoxLang tools:

<table><thead><tr><th width="165">Folder</th><th>Description</th></tr></thead><tbody><tr><td><strong>browser</strong></td><td>This is a little utility to facilitate navigating big testing suites. This helps navigate to the suites you want and execute them instead of typing all the time.</td></tr><tr><td><strong>tests</strong></td><td>A vanilla test runner for any application</td></tr><tr><td><strong>runner</strong></td><td>A simple GUI test runner</td></tr></tbody></table>

### IDE Tooling

Now that you are installed, please set up your favorite IDE with our tooling extensions so it can make your testing experience more enjoyable.

{% content-ref url="ide-tools.md" %}
[ide-tools.md](ide-tools.md)
{% endcontent-ref %}

## TestBox CLI

TestBox comes with its own CLI for CommandBox.  You can use it to generate tests, harnesses, and suites and also run executions from the CLI.

```bash
install testbox-cli
```

You will now have the `testbox` namesapce available to you, try it out

```bash
testbox help
```

### Generating a Testing Harness

Once you install TestBox, you'll need a quick way to set up a testing harness in your project. The `generate harness` command will add a new `/tests` folder to your application with a few example tests to get you started.

```bash
testbox generate harness
```

`/tests` - The test harness generated

* `/resources` - Where you can place any kind of testing helpers, data, etc
* `/specs` - Where your test bundles can go
  * `/unit`
  * `/integration`
* `Application.bx|cfc` - Your test harness applilcation file. Controls life-cycle and application concerns.
* `runner.bx|cfm` - A web runner template that executes ALL your tests with tons of different options from a running web server.
* `test.xml` - An ANT task to do JUNIT testing.

You can then run your tests by executing the `testbox run` command in CommandBox or by visiting the web runner in the generated harness: `http://localhost/tests/runner.cfm`

```bash
testbox run --help
```

### Generating Tests

You can also use the CLI to generate tests according to your style and also it detects if you are using BoxLang or a CFML engine.

```bash
testbox create bdd --help
testbox create unit --help
```

### Language Generation

If you want to be explicit you can use the `language` id to set it to your preferred language for generation and usage.  We recommend this so all CLI tooling can detect what language your project is in.

```bash
# For BoxLang Generation
package set language="boxlang"

# For CFML
package set language="cfml"
```
